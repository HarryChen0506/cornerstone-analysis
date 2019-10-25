# 如何实现缓存机制

Cornerstone设计的图片缓存机制，在用户切换图片时起到非常关键的作用。区别于其他的图形图像库，例如Papaya,后者需要提前获取一组DICOM图像，或者一个nifti文件包（包含多张DICOM），这样会导致初次加载耗时非常长，用户体验不佳。Cornerstone作为基础的2D图像库，它每次只处理单张imageId的数据，任何切换图像的操作需要重新调用 `loadImage`接口。因此如果没有良好的缓存机制，则不能流畅的进行图片切换。

loadImage
```
// 代码略有改动，只展示了核心原理，具体步骤忽略
export function loadImage (imageId, options) {
  // 从缓存里获取数据
  const imageLoadObject = getImageLoadObject(imageId);
  if (imageLoadObject !== undefined) {
    return imageLoadObject.promise;
  }
  // 如果缓存里没有，则从loader里直接获取
  return loadImageFromImageLoader(imageId, options).promise;
}
```
imageCache.js
```
let maximumSizeInBytes = 1024 * 1024 * 1024; // 1 GB
let cacheSizeInBytes = 0;
const imageCacheDict = {};
export const cachedImages = [];

// 获取缓存
export function getImageLoadObject (imageId) {
  const cachedImage = imageCacheDict[imageId];
  if (cachedImage === undefined) {
    return;
  }
  return cachedImage.imageLoadObject;
}
// 当从image loader里获取数据后，则保存到缓存中
export function putImageLoadObject (imageId, imageLoadObject) {
  const cachedImage = {
    loaded: false,
    imageId,
    sharedCacheKey: undefined, // The sharedCacheKey for this imageId.  undefined by default
    imageLoadObject,
    timeStamp: Date.now(),
    sizeInBytes: 0
  }
  // 字典的方式保存
  imageCacheDict[imageId] = cachedImage;
  cachedImages.push(cachedImage)
  imageLoadObject.promise.then(function (image) {
    cachedImage.loaded = true;
    cachedImage.image = image;
    cachedImage.sizeInBytes = image.sizeInBytes;
    cacheSizeInBytes += cachedImage.sizeInBytes;
    const eventDetails = {
      action: 'addImage',
      image: cachedImage
    };
    purgeCacheIfNecessary();
  })
}
```