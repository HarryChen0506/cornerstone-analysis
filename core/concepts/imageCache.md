# Image Cache

> Cornerstone stores Images inside the Image Cache to keep track of memory usage.

Cornerstone 在切换渲染图像方面比较流畅的一个因素就是，设计了良好的图片缓存机制。

当图像加载器返回一个Promise时，图像对象将会被存储在图片缓存模块中，它被作为least-recently-used (LRU，最近最少使用）缓存进行的设置。

最初调用loadImage时，缓存将填充缓存图像的占位符，记录大小为0。当获取到图像加载后的promise，记录的大小将以字节为单位以实际大小更新。如果加载失败，则从缓存中删除占位符。

开发者可以做到：

* 设置最大缓存大小，默认1 GB
* 手动清除任意图像的内存
* 获取缓存的统计信息
* 更改任意图片的缓存大小

