# 如何实现坐标系转换？

我们先来看一个在图像上画一个几何图形标注的🌰。

![image](/assets/images/custom-marker.jpg)

``` 
    const element = document.getElementById('dicomImage');
    cornerstone.enable(element);
    function onImageRendered(e) {
        const eventData = e.detail;
        // 将canvas context设置成图片坐标系
        cornerstone.setToPixelCoordinateSystem(eventData.enabledElement, eventData.canvasContext);
        // 在像素坐标系下，绘制（0,0）将会在图片的左上角的，而（rows, columns）则是图片的右下角，即最大的行数和列数。
        const context = eventData.canvasContext;
        context.beginPath();
        context.strokeStyle = 'white';
        context.lineWidth = .5;
        context.rect(128, 90, 50, 60);
        context.stroke();
        context.fillStyle = "white";
        context.font = "6px Arial";
        context.fillText("Tumor Here", 128, 85)
    }
    element.addEventListener('cornerstoneimagerendered', onImageRendered);
    const imageId = 'example://1';
    cornerstone.loadImage(imageId).then(function (image) {
        cornerstone.displayImage(element, image);
    });
```

为了实现在图片上添加一个标记这样的需求，我们要先将canvas的坐标系转换成像素坐标系。cornerstone.setToPixelCoordinateSystem( )这个接口可以实现这样的坐标系转换。
我们来看看setToPixelCoordinateSystem是如何实现的

``` 
/* setToPixelCoordinateSystem.js */
import calculateTransform from './internal/calculateTransform.js';
export default function (enabledElement, context, scale) {
  const transform = calculateTransform(enabledElement, scale);
  context.setTransform(transform.m[0], transform.m[1], transform.m[2], transform.m[3], transform.m[4], transform.m[5]);
}
/*calculateTransform.js*/
// 代码略有改动，只展示了核心原理，具体步骤忽略
export default function (enabledElement, scale) {
  const transform = new Transform();
  // 移到canvas中心
  transform.translate(enabledElement.canvas.width / 2, enabledElement.canvas.height / 2);
  // 获取图片旋转角度
  const angle = enabledElement.viewport.rotation;
  // 获取缩放比例
  let widthScale = enabledElement.viewport.scale;
  let heightScale = enabledElement.viewport.scale;
  // 设置scale transform
  transform.scale(widthScale, heightScale);
  // 设置 transform translate
  transform.translate(enabledElement.viewport.translation.x, enabledElement.viewport.translation.y);
  // 设置scale rotate
  if (angle !== 0) {
    transform.rotate(angle * Math.PI / 180);
  }
  return transform;
}
```
canvas 通过transform接口用矩阵处理的方式，实现对坐标系的旋转位移缩放操作。setTransform( )方法会重置前面的矩阵，然后再绘制出一个新的矩阵；transform( )方法则不会重置前面的矩阵，而是在前面的基础上继续进行缩放、位移或者旋转。我们的Cornerstone则利用了setTransform来重置当前的像素坐标系。在每次图片渲染后，通过图片的viewport，可以计算出transform的矩阵参数，在重置当前的像素坐标系后，那么新绘制的标记（相对像素的坐标）则可以正确的进行定位。

