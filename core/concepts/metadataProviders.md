# Metadata Providers

> A Metadata Provider is a JavaScript function that acts as an interface for accessing metadata related to Images in Cornerstone. Users can define their own provider functions in order to return any metadata they wish for each specific image.

Cornerstone 提供一个Metadata Provider这样的架构来帮助用户获取图片的metadata。开发者通过自定义自己的provider纯函数，通过调用方法即可返回自己所需的meta数据.

医学图像通常携带很多非像素信息的元数据，例如图像的像素间距、患者ID或扫描时间。对于某些文件类型(如DICOM), 这些信息存储在文件头部，可以被读取、解析和传递。然而像JPEG, PNG这样的图像类型，他们不像医学图像，需要单独提供这些信息。当然对于DICOM图像，开发人员也可以自行提供元数据，而不需要从服务器向客户端传输像素数据，因为这样可以大大提高性能。

为了处理这些场景，Cornerstone 提供了定义和使用Metadata Providers的基础结构。Metadata Providers 其实就是一个简单的纯函数，入参是Image Id和metaData类型Type, 返回一个metadata具体信息。

## 补充

* Cornerstone 允许注册多个Metadata Providers
* 每个provider能给与开发者任何想要的信息
* 当针对图像的元数据进行请求时，Cornerstone将遍历已知的提供者，直到它获得了指定元数据类型的已定义的元数据集。
* Providers可以设置优先级，为了影像被调用的顺序
* 当 DICOM图像被Cornerstone WADO Image Loader加载时，他们的元数据会被自动的解析并且加入到metadata provider里。
* 在 Cornerstone Tools中，特定的元数据类型被用来为某些工具提供元数据

这里有一个简单的🌰，返回Image Plane metadata信息。

``` 
function metaDataProvider(type, imageId)
  if (type === 'imagePlaneModule') {
    if (imageId === 'ct://1') {
        return {
            frameOfReferenceUID: "1.3.6.1.4.1.5962.99.1.2237260787.1662717184.1234892907507.1411.0",
            rows: 512,
            columns: 512,
            rowCosines: {
                x: 1,
                y: 0,
                z: 0
            },
            columnCosines: {
                x: 0,
                y: 1,
                z: 0
            },
            imagePositionPatient: {
                x: -250,
                y: -250,
                z: -399.100006
            },
            rowPixelSpacing: 0.976562,
            columnPixelSpacing: 0.976562
        };
    }
  }
}
// 注册 provider
cornerstone.metaData.addProvider(metaDataProvider);
// 获取 metaData
var imagePlaneModule = cornerstone.metaData.get('imagePlaneModule', 'ct://1');
```

Cornerstone在metaData模块里的getMetaData，会迭代所有注册的providers，一旦发现执行provider函数返回的结果不为空，则说明已经获取了用户所想要的metadata信息。通常在某些第三方库的Image Loader里已经写好一些provider并注册到cornerstone里了。

``` 
// Cornerstone暴露的获取metaData api
function getMetaData (type, imageId) {
  // Invoke each provider in priority order until one returns something
  for (let i = 0; i < providers.length; i++) {
    const result = providers[i].provider(type, imageId);
    if (result !== undefined) {
      return result;
    }
  }
}
```

