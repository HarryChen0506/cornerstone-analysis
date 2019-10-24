# Image Loaders

> An Image Loader is a JavaScript function that is responsible for taking an Image Id for an Image and returning the corresponding Image Load Object for that Image to Cornerstone. The Image Load Object contains a Promise which resolves to produce an Image.

一个Image Loader js插件库的作用就是，从一个Image Id入参，返回一个 Image Load Object给Cornerstone。这个Image Load对象包含一个Promise，可以返回Image信息。因为加载一个图片，需要向后端发送请求，这个过程往往是异步的，所以返回一个Promise有利于代码的组织。

## 图片加载流程

![image](/assets/images/image-loader-workflow.png)

1. ImageLoaders在Cornerstone上注册自己时，指定好特定的ImageId URL schemes。
2. 应用通过loadImage()接口来请求加载某张图片.
3. Cornerstone将请求代理到ImageLoader, 通过ImageId区分出URL schemes，进而选择对应的imageLoader.
4. ImageLoader一旦加载文件，获取像素信息后，会立刻返回一个含有Promise的image Object.获取像素信息需要调用http层的XHR，或俗称ajax,获取的数据甚至还要做解压处理，有时还要再转换成Cornerstone能够理解的图像格式。
5. 通过返回的Promise,调取displayImage()接口即可实现图像的渲染

```
// 注册一个image Loader,使用'myCustomLoader'作为url scheme 
cornerstone.registerImageLoader('myCustomLoader', loadImage);

// 当解析imageId时获取的url scheme为'myCustomLoader', 则调用myCustomLoader去完成加载等下面的流程
cornerstone.loadImage('myCustomLoader://example.com/image.dcm')
```
### 可使用的Image Loaders

| Image Loader                  | Used for                                                                                                                        |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Cornerstone WADO Image Loader | DICOM Part 10 images <br> 支持 WADO-URI and WADO-RS <br> 支持 multi-frame DICOM instances <br> 支持 从File objects里阅读 DICOM 文件 |
| Cornerstone Web Image Loader  | PNG and JPEG images                                                                                                             |

## Image Load Object

为什么Image Loader返回的是一个Image Object而不是直接的一个Promise, 原因是返回的对象可以补充其他的属性，为将来的扩展打好基础。

## 如何写一个自定义的Image Loader

``` 
function loadImage(imageId) {
    // 解析ImageId, 获取url
    const url = parseImageId(imageId);
    const promise = new Promise((resolve, reject) => {
     // 创建XHR对象
      const oReq = new XMLHttpRequest();
      oReq.open("get", url, true);
      oReq.responseType = "arraybuffer";
      oReq.onreadystatechange = function(oEvent) {
          if (oReq.readyState === 4) {
              if (oReq.status == 200) {
                  // 请求成功，获取到文件信息，解析后再创建成image object
                  const image = createImageObject(oReq.response);
                  resolve(image);
              } else {
                  reject(new Error(oReq.statusText));
              }
          }
      };
      oReq.send();
    })
   // 返回带有promise的对象
    return {
      promise
    };
}
```

