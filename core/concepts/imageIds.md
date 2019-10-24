# Image Ids

> A Cornerstone Image Id is a URL which identifies a single image for cornerstone to display.

Image Id其实就是一个特定组成的图片的id。

Image ID的结构参考下图:

![image](/assets/images/image-id-format.png)

 URL scheme被用来决定调用目标加载器, 它和图片url地址之间被: 号隔离开来。这种策略可以使Cornerstone同时通过不同的加载器展示不同类型的图片。例如通过WADO Loader展示一个DICOM类型的CT图片的同时，还可以支持展示JPEG类型的普通皮肤图片。

Cornerstone不会特意指定URL的格式，这取决于相应的Image Loader的支持和规定。下面是一些Image Loader插件典型的Image Id：

* example://1
* dicomweb://server/wado/{uid}/{uid}/{uid}
* http://server/image.jpeg
* custom://server/uuid

