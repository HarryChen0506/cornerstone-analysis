# Viewports

> Each Enabled Element has a Viewport which describes how the Image should be rendered.

Viewport描述了一个图像在canvas上的如何渲染的状态值，比如旋转，移动等参数。对于enabled element，可以通过 getViewport() 获取viewport信息， setViewport()接口设置viewport.

我们来看一个viewport参数的🌰

``` 
{
	"scale": 2, // 缩放
	"translation": { // 位移
		"x": 0,
		"y": 0
	},
	"voi": { // 窗宽窗位
		"windowWidth": 256,
		"windowCenter": 127
	},
	"pixelReplication": false,
	"rotation": 0, // 旋转
	"hflip": false, // 左右翻转
	"vflip": false, // 上下翻转
	"labelmap": false, // 标签
	"displayedArea": { //展示区域
		"tlhc": {
			"x": 1,
			"y": 1
		},
		"brhc": {
			"x": 256,
			"y": 256
		},
		"rowPixelSpacing": 0.8984375, // 像素行间距
		"columnPixelSpacing": 0.8984375, // 像素列间距
		"presentationSizeMode": "NONE"
	}
}
```

