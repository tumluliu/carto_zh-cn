### 4.7 实例：制作专题地图

在本章的最后，我们看一个10分钟的制图实例教程，它将演示如何利用QGIS生成热图，以及如何在TileMill中将其制图可视化。

#### 使用QGIS

在开始工作之前，请确认你使用的QGIS版本至少是1.9，这是撰写本文档时的最新开发版本（译注：在整理译稿时的最新稳定版本是2.8）。Mac用户可以从[kyngchaos](http://www.kyngchaos.com/software/qgis)下载到最新版（译注：也可以通过Homebrew安装）。

接下来你需要激活QGIS中的Heatmap插件。方法是从Plugins \> Manage plugins菜单中打开如下面板，然后在Heatmap插件前面的复选框打上勾。_注意：这个插件只有在1.9以上版本的QGIS中才可以使用。_重启QGIS使该设置生效。

![](http://farm6.staticflickr.com/5325/7173395958_5d4d96aef9_z.jpg)

现在需要加入一些点数据。下面这个数据集由华盛顿特区酒精饮料管理局提供，发布于华盛顿特区开放数据门户网站[data.dc.gov](http://data.dc.gov/)。数据的下载地址在[这里](http://dcatlas.dcgis.dc.gov/download/ABRALicenseePt.ZIP)。

![](http://farm8.staticflickr.com/7076/7173395998_9f16de7e40_z.jpg)

依次进入菜单Raster \> Heatmap \> Heatmap，然后可以看到如下对话框：

![](http://farm9.staticflickr.com/8003/7173395914_a451f48105_z.jpg)

指定输出文件的路径（带.tif后缀），暂时保持其它的设置为默认值，然后点击`OK`按钮即可生成热图。此时项目中会多出一个新的图层，它是一个巨大的灰度矩形。打开这个新图层的属性面板可以看到它不同的颜色值。为了快速得到一个粗糙的可视化效果，我们使用QGIS中内置的一个配色模板。在Style标签页的Colormap下拉框中选中`Pseudocolor`并点击`OK`，就可以得到一幅如下效果的地图：

![](http://farm6.staticflickr.com/5040/7173396034_7f38edb250_z.jpg)

既然已经得到了这幅地图，那么我们就可以再试试调整一下它的颜色和其它参数。默认的配置简单是简单，但却很难将数据表现得赏心悦目。在Heatmap对话框的帮助中，有关于“空间缓冲区”和“衰变率”的解释。而在前一节“栅格制图技巧”中，也讲述了如何使用自定义的配色方案来渲染栅格数据。为了能够在接下来的步骤中使用TileMill制图并制作瓦片，需要保证输出文件的格式为GeoTIFF。

在TileMill中新建一个项目，把之前QGIS中保存的GeoTIFF文件作为一个图层加载进来。删掉创建项目时默认加入的`#countries`层，修改地图背景，得到如下CartoCSS代码：

	
	#heatmap {
	  raster-opacity: 1;
	  raster-scaling: bilinear;
	}
	

此时地图应该已经出现了。关于栅格数据样式的配置请参考前节内容。参考[AJ](https://twitter.com/#!/aj_ashton)设计的[世界人口分布图](http://www.flickr.com/photos/developmentseed/6286976630/in/photostream/lightbox/)可以得到更多灵感。

#### 用TileMill实现伪热图

TileMill won’t generate rasterized heatmaps like the QGIS plugin can, but you can approximate the effect with a few CartoCSS tricks to take advantage of aggregated opacity: low opacity of individual points means that overlap in dense areas has a stronger, more saturated color value.

TileMill不会像QGIS插件一样生成栅格化的热图。但是利用一些CartoCSS技巧，可以实现近似的效果。这些技巧包括聚合透明度：低透明度的点意味着在密集区域相互叠加会得到强度（译注：stronger是指什么？）和饱和度都更高的颜色值。

对原始矢量数据shapefile使用如下几行CartoCSS即可得到所需的效果：

	
	#abralicenseept [DESCRIPTIO != 'Retailer B']{
	  marker-width:4;
	  marker-fill:#ef0;
	  marker-opacity:.45;
	  marker-line-opacity:0;
	  marker-allow-overlap:true;
	} 
	

_注意：这段代码忽略了所有’Retailer B’，也就是去掉了那些杂货店点。这样可以更好的反映出华盛顿特区中酒吧的位置，以及夜生活的分布情况。_

这种方法的好处在于能够在地图放大到较高级别时保留和展示每个点的精确信息，而且还可以支持要素级的交互。更多关于样式配置方面的内容，可以参见本章中“高级地图设计”一节。

#### 参考文献

1. Mapbox, [Designing Heat Maps](https://www.mapbox.com/tilemill/docs/guides/designing-heat-maps/)