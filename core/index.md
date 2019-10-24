# Cornerstone Core

> Cornerstone Core, which is a lightweight JavaScript library for displaying medical images in modern web browsers that support the HTML5 canvas element. Cornerstone Core is not meant to be a complete application itself, but rather a component that can be used as part of larger, more complex applications.

[Cornerstone Core](https://github.com/cornerstonejs/cornerstone.git) 是一个支持在现代浏览器上展示医疗影像的轻量级js库，图像绘制可以选择使用cancas或webGL。Cornerstone Core作为Cornerstone的核心库，它仅仅作为最底层的渲染组件。如果需要构建大型应用，还需要借助cornerstone配套的相关库如CornerstoneTools等提供交互功能。如果想体验复杂应用的话，可以前往 [OHIF Viewer](https://viewer.ohif.org/)

Cornerstone Core对于展示图像的容器没有限制，同时也是不可知的，这样用户可以对任意html元素如div做enable处理，赋予他们的展示图像的能力，其实也就是在容器内部创建一个canvas元素。另外，Cornerstone Core也没有读取和解析图片文件的能力，Image Loader的功能完全交给了类似[cornerstoneWADOImageLoader](https://github.com/cornerstonejs/cornerstoneWADOImageLoader)这样的DICOM图片加载器, 这种解耦方式的好处就是，开发者可以根据不同的图片类型做相应处理，甚至自定义自己的图片加载器。

在图像交互方面，Cornerstone Core对用户操作也是不可知的。它仅仅关心图片参数的终态，至于操作方式比如鼠标、触摸还是键盘实现的缩放、移动等操作，它交给了[cornerstoneTools](https://github.com/cornerstonejs/cornerstoneTools)这个库去实现了。

对于一些基础概念，本书将会直接参考[官网](https://docs.cornerstonejs.org/)的介绍，并加入作者的一些理解，建议读者可以先去阅读下他们官网的文档。后面的话，作者将会针对一些关键性的地方，做一些源码解析。
