# 认识cornerstone

> Cornerstone is an open source project with a goal to deliver a complete web based medical imaging platform.

Cornerstone 是一个开源项目，旨在提供一个完全基于web的医疗影像平台。

Cornerstone早期的时候还是一个单一的完整的项目，但是随着项目的复杂性逐渐增大，单项目的架构已经不足以支持项目的发展。

![image](/assets/images/library-hierarchy.png)

目前，Cornerstone生态的核心库要属Cornerstone Core，它是一个集图像渲染、加载、缓存和视口变换的library，通常也被称为Cornerstone。除了核心库之外，Cornerstone开发团队还支持了其他几个库，这些库为开发复杂的成像应用程序提供了生态系统。

| js库                          | 描述                                  |
|-------------------------------|---------------------------------------|
| Cornerstone Core              | 核心库，提供图像渲染，加载，缓存和视口变换     |
| Cornerstone Tools             | 可扩展的上层工具库，支持鼠标，键盘和触摸设备  |
| Cornerstone WADO Image Loader | 针对 DICOM Part 10 files 文件的图像加载器 |
| Cornerstone Web Image Loader  | 针对网络图片 (PNG, JPEG) 的图像加载器     |
| Cornerstone Math              | 数学或函数工具                          |
| dicomParser                   | DICOM Part 10 文件解析器               |

