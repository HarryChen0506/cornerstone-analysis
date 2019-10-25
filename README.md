# 前言

身为一名前端开发者，时常会听到圈内的朋友抱怨"学不动了"。确实如此，前端技术的横向发展和迭代速度实在是太快了，然而人的精力却是有限的，在中高级的技术进阶阶段，广撒网式的学习方式往往会适得其反。那些调侃程序员的中年危机的段子，说不好哪天真的变成了现实，那么前端er到底该如何构建自己的技术护城河？

就目前看来，前端发展的主要领域集中在：工程化、音视频处理、服务端（中间层或服务端渲染）、多端化（小程序，RN等）、以及本文即将介绍的图形图像处理。选择一个细分领域进行聚焦，持续深入的耕耘, 或许是个不错的应对之策。

说起图形图像处理，大家很容易联想到canvas实现的数据可视化或动画特效、openGL或webGL实现的三维模型等。从2015年开始，在AI算法和框架的引爆下，人脸识别等领域的商业化应用相继落地。与此同时，在医疗领域同样也掀起了一场革命，人工智能借助医疗影像大数据及图像识别技术，在肺结节、眼底、乳腺癌、宫颈癌等方面已取得了较为成熟的产品。

AI医疗井喷式的发展使得AI算法工程师炙手可热外，医疗软件研发和系统工程师也成为了下半场最抢手的人才。原因在于:

> 这里的软件工程师，往往要求是具备一定医疗软件开发经验的。放射科检查流程、医学影像基础原理、DICOM协议，这些都要多少知道一些。另外，在一个商业产品化的医疗影像分析诊断软件中，AI算法（深度学习模型）其实只占整个系统的20~30%，或更少。AI算法是整个系统中技术含量最高的部分，但绝对不是工作量最大的部分，也不是研发投入最大的部分。现在的软件基本都是网络化，从服务器端，到网页端或客户端，必然包含UI、通讯、数据库、存储等一系列必须开发的组件或基础架构。这些组件的开发往往要整个软件系统的70%或更多。这里，还没有包括持续发布、自动化测试、安装包等辅助基础设施的搭建。所以，现在几乎每个AI公司都已经成立了专门的软件组（部）---[《医疗影像AI下半场，什么人才最抢手？》](https://zhuanlan.zhihu.com/p/52621172)


![image](/assets/images/timg.jpeg) 

( 图片来源于网络，如有侵权，请联系删除！)

> 正是借助开源软件，初创AI公司才能够快速推出自己的软件产品，获得进入市场跑道，参与竞争的机会。说的直白一些，如果没有Tensor Flow和PyTorch这样的深度学习库，很多公司不可能在短短1~2年间推出自己的软件产品。这里还不能忽视Cornerstone这类软件的给力助攻。---[《是巨人的肩膀，还是长江里的后浪》](https://zhuanlan.zhihu.com/p/80381961)

这里所提及的[Cornerstone](https://github.com/cornerstonejs/cornerstone), 是一套基于javascript语言实现的医疗影像库，正如其名"基石", 它的诞生, 使得在浏览器的web上显示高清医学影像成为可能，要知道此前的这类应用只能由开发客户端的C##等语言才能实现。cornerstone除了处理图像移动、缩放、旋转、标注、测量、滤镜、图片融合等基础功能外，还提供了医学图像特有的窗宽窗位调整，借助其他生态配套库还可以实现MPR(多平面重建)。

Cornerstone开源库的核心团队为了打造Cornerstone最佳实践，同时也创建了一个医疗阅片系统项目[《OHIF Viewer》](https://viewer.ohif.org/)（界面见下图）。

> 随着Web前端等软件开发技术的出现，软件开发的整体趋势是成本下降、周期缩短。类似上面的功能，传统公司早年可能要用12人月的成本开发出来。但新兴公司可能只需要3人月就可以开发出来。而且界面美观和时尚程度，可能还要远超现有传统产品。简单来说，护城河仍然存在，但传统厂家花10年挖的护城河，新势力借助开源软件助攻，花1~2年可能就可以跨越。---《是巨人的肩膀，还是长江里的后浪》

为了深入理解其原理, 笔者目前正在研究Cornerstone以及相关生态库的源码，开发之余将源码解析整理成了笔记[《Cornerstone源码解析》](https://harrychen0506.github.io/cornerstone-analysis/)，本着开源的精神，和大家一起来分享。由于web医疗影像领域偏小众化，缺乏良好的交流平台，所以笔者在文末提供一个微信群，希望大家可以在里面相互交流，解决实际问题，这也是笔者想写这篇文章的另一个目的。

| ![image](/assets/images/wechat.png) | ![image](/assets/images/gzh.png) |
| --- | ---|

### 相关链接

* [《从Cornerstone到OHIF Viewer—JS影像前端开发最佳范例》](https://zhuanlan.zhihu.com/p/58767457)
* [《是巨人的肩膀，还是长江里的后浪》](https://zhuanlan.zhihu.com/p/80381961)
* [《医疗影像AI下半场，什么人才最抢手？》](https://zhuanlan.zhihu.com/p/52621172)
* [《Cornerstone源码解析》](https://harrychen0506.github.io/cornerstone-analysis/)

