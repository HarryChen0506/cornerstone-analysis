# 渲染过程发生了什么

Cornerstone 是如何将图片渲染在canvas上的？
Cornerstone对某个元素进行enable(element)操作后，将会生成一个`canvas`，创建出一个新的`enabledElement`对象。与此同时，将会启动帧循环检测`requestAnimationFrame`，该渲染循环对内部做了判断，当`enabledElement`需要重绘`needsRedraw`时，即可进入`drawImageSync(enabledElement, enabledElement.invalid)`的流程。

enable.js
```
export default function (element, options) {
  const canvas = getCanvas(element);
  const enabledElement = {
    element,
    canvas,
    image: undefined, 
    invalid: false,
    needsRedraw: true, // 是否需要重绘
    options,
    layers: [],
    data: {},
    renderingTools: {},
    uuid: generateUUID()
  };
  addEnabledElement(enabledElement)
  function draw (timestamp) {
    const eventDetails = {
      enabledElement,
      timestamp
    }
    if (enabledElement.needsRedraw) {
      drawImageSync(enabledElement, enabledElement.invalid);
    }
    requestAnimationFrame(draw);
  }
  draw();
}
```
drawImageSync.js
```
import { renderGrayscaleImage } from '../rendering/renderGrayscaleImage.js';
export default function (enabledElement, invalidated) {
  const image = enabledElement.image;
  const element = enabledElement.element;

  render = renderGrayscaleImage;  
  // 灰度图像的渲染
  render(enabledElement, invalidated);
  enabledElement.invalid = false;
  enabledElement.needsRedraw = false;
}
```
renderGrayscaleImage.js
```

export function renderGrayscaleImage (enabledElement, invalidated) {
  const image = enabledElement.image;
  // 获取 canvas context 和重置transform
  const context = enabledElement.canvas.getContext('2d');
  context.setTransform(1, 0, 0, 1, 0, 0);
  // 清除画布
  context.fillStyle = 'black';
  context.fillRect(0, 0, enabledElement.canvas.width, enabledElement.canvas.height);
  // 利用viewport参数重置像素坐标系
  setToPixelCoordinateSystem(enabledElement, context);
  // 图片canvas
  let renderCanvas = getRenderCanvas(enabledElement, image, invalidated);
  const sx = enabledElement.viewport.displayedArea.tlhc.x - 1;
  const sy = enabledElement.viewport.displayedArea.tlhc.y - 1;
  const width = enabledElement.viewport.displayedArea.brhc.x - sx;
  const height = enabledElement.viewport.displayedArea.brhc.y - sy;
  // 绘制新的图片
  context.drawImage(renderCanvas, sx, sy, width, height, 0, 0, width, height);
}
```