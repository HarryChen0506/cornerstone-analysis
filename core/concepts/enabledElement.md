# Enabled Element

> In Cornerstone, an Enabled Element is an HTMLElement (typically a div) which we display an interactive medical image inside of.

Enabled Element 就是一个可以展示和交互医疗影像的容器。通过特定的接口，cornerstone给指定的容器赋能，将enabledElement纳入了cornentstone的管理中。

为了展示图像，开发者需要遵循以下步骤：

* 引入Cornerstone js库，可以通过全局标签 `<script>` ，也可以通过 `import` 的es6方式。
* 引入Image Loaders，例如 WADO, WADO-RS或自定义加载器
* 创建HTML DOM元素作为展示图像的容器，通过css或style设置理想的宽和高
* 调用enable() API 来给元素赋能
* 调用loadImage() API来加载图片文件
* 加载后文件包含了图片的像素信息，此时可以使用displayImage() API来渲染图片了

一个最简单的🌰如下

``` 
const imageId = 'example://1'
const element = document.getElementById('dicomImage')
cornerstone.enable(element)
cornerstone.loadImage(imageId).then(function(image) {
  // image 为文件加载并解析好的对象
  cornerstone.displayImage(element, image)
});
```

