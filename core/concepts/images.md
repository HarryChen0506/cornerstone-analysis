# Images

> Image Loaders return an Image Object.

Image Loader 返回的Image 对象的🌰

``` 
{
	"imageId": "example://1", // 图片id
	"minPixelValue": 0, // the minimum stored pixel value
	"maxPixelValue": 257, // the maximum stored pixel value
	"slope": 1, // 将像素值转换为模态像素值的重缩放斜率
	"intercept": 0, // 将像素值转换为模态像素值的重缩放截距
	"windowCenter": 127, // 窗位
	"windowWidth": 256, // 窗宽
	"rows": 256, // 行数
	"columns": 256, // 列数
	"height": 256, // 高度
	"width": 256, // 宽度
	"color": false, // true is RGB, false if grayscale
	"columnPixelSpacing": 0.8984375, // 列像素间距
	"rowPixelSpacing": 0.8984375, // 行像素间距
	"sizeInBytes": 131072, // 字节
	"stats": { // 上次重画图像的统计信息
		"lastGetPixelDataTime": 0.025000001187436283,
		"lastStoredPixelDataToCanvasImageDataTime": 4.854999999224674,
		"lastPutImageDataTime": 0.06500000017695129,
		"lastRenderTime": 12.930000000778819,
		"lastLutGenerateTime": 0.38999999742372893
	},
	"cachedLut":{} // 缓存查找表
}
```

