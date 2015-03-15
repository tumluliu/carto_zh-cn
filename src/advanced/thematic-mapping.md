### 实例：制作专题地图

_译注：[原文地址](https://www.mapbox.com/tilemill/docs/guides/designing-heat-maps/)_

This is a 10 minute walk through showing how to generate heat maps in QGIS and then display them in TileMill.

这是一个10分钟的实例教程，它将演示如何利用QGIS生成热图，以及如何在TileMill中将其制图可视化。

#### 使用QGIS（Working with QGIS）

Make sure you are running at least QGIS \>= 1.9. At the time of writing, this is the development release. Mac users can find it at [kyngchaos](http://www.kyngchaos.com/software/qgis).

在开始工作之前，请确认你使用的QGIS版本至少是1.9，这是撰写本文档时的最新开发版本（译注：在整理译稿时的最新稳定版本是2.8）。Mac用户可以从[kyngchaos](http://www.kyngchaos.com/software/qgis)下载到最新版。

Next you’ll need to enable the Heatmap Plugin. Open the pane below by selecting Plugins \> Manage plugins. In the following pane, check the box for the Heatmap Plugin. _Note: This plugin is only available for QGIS 1.9 and above_. You may need to restart QGIS for these changes to take effect.

接下来你需要激活QGIS中的Heatmap插件。方法是从Plugins \> Manage plugins菜单中打开如下面板，然后在Heatmap插件前面的复选框打上勾。_注意：这个插件只有在1.9以上版本的QGIS中才可以使用。_重启QGIS使该设置生效。

![](http://farm6.staticflickr.com/5325/7173395958_5d4d96aef9_z.jpg)

Now add some point data. Data below is from the Washington, DC Alcoholic Beverage Regulation Administration, made available by DC’s open data portal [data.dc.gov](http://data.dc.gov/). It can be downloaded [here](http://dcatlas.dcgis.dc.gov/download/ABRALicenseePt.ZIP).

现在需要加入一些点数据。下面这个数据集来自华盛顿特区酒精饮料管理局的开放数据门户[data.dc.gov](http://data.dc.gov/)。下载地址在[这里](http://dcatlas.dcgis.dc.gov/download/ABRALicenseePt.ZIP)。

![](http://farm8.staticflickr.com/7076/7173395998_9f16de7e40_z.jpg)

Now go to Raster \> Heatmap \> Heatmap. You should see a dialog box like this:

依次进入菜单Raster \> Heatmap \> Heatmap，然后可以看到如下对话框：

![](http://farm9.staticflickr.com/8003/7173395914_a451f48105_z.jpg)

Specify an output filepath (with a .tif extension) and leave the default settings for now. Hit `OK` to generate your heat map. A big gray rectangle will be added to the project as a new layer. To see the different color values, open the properties pane for your new layer. To get a quick and dirty visualization of the different values we’ll use one of the QGIS preset color schemes. Under the Style tab, set the Colormap drop down menu to ‘Pseudocolor’ and hit ok. You should see a map like this:

指定输出文件的路径（带.tif后缀），暂时保持其它的设置为默认值，然后点击`OK`按钮即可生成热图。此时项目中会多出一个新的图层，它是一张巨大的灰度矩形。打开这个新图层的属性面板可以看到它不同的颜色值。为了快速得到一个粗糙的可视化效果，我们使用QGIS中内置的一个配色模板。在Style标签页的Colormap下拉框中选中`Pseudocolor`然后点击`OK`，然后可以得到一幅如下效果的地图：

![](http://farm6.staticflickr.com/5040/7173396034_7f38edb250_z.jpg)

Now that you have your map, play around with the colors and other parameters. Defaults are easy, but rarely expose data well. The Heatmap dailog has a great help section that painlessly explains what a ‘spatial buffer’ and ‘decay ratio’ are. See an [earlier post](http://mapbox.com/blog/visualizing-global-forest-height/) on working with raster data to come up with your own custom color scheme. If you want to render your map to tiles with TileMill, the GeoTIFF default is one you don’t have to change.

既然已经得到了这幅地图，那么我们就可以再试试调整一下它的颜色和其它参数。默认的配置简单是简单，但却很难将数据表现得赏心悦目。在Heatmap对话框的帮助中，有关于“空间缓冲区”和“衰变率”的解释。而在前一节“栅格制图技巧”中，也讲述了如何使用自定义的配色方案来渲染栅格数据。如果要在TileMill中渲染地图并制作瓦片，那么请保持GeoTIFF格式不变。

To render in TileMill, open a new project and load your .tiff as a new layer. Get rid of the `#countries` layer, and change the map background so that your carto looks like this:

在TileMill中新建一个项目，把之前QGIS中保存的GeoTIFF文件作为一个图层加载进来。删掉创建项目时默认加入的`#countries`层，修改地图背景，得到如下CartoCSS代码：

	
	#heatmap {
	  raster-opacity:1;
	  raster-scaling: bilinear;
	}
	

Your map should appear. Just hit upload in the [export](http://mapbox.com/tilemill/docs/crashcourse/exporting/) menu to start sharing your map online. For more info on working with raster data, read docs on [reprojecting GeoTIFFs](http://mapbox.com/tilemill/docs/guides/reprojecting-geotiff/) and [working with terrain data](http://mapbox.com/tilemill/docs/guides/terrain-data/). If you want more inspiration, check out some of previous work like [AJ’s](https://twitter.com/#!/aj_ashton) map of the [world population](http://www.flickr.com/photos/developmentseed/6286976630/in/photostream/lightbox/).

此时地图应该已经出现了。关于栅格数据样式的配置请参考前节内容。参考[AJ](https://twitter.com/#!/aj_ashton)设计的[世界人口图](http://www.flickr.com/photos/developmentseed/6286976630/in/photostream/lightbox/)可以得到更多灵感。

#### 用TileMill实现伪装热图（Faking it with TileMill）

TileMill won’t generate rasterized heatmaps like the QGIS plugin can, but you can approximate the effect with a few CartoCSS tricks to take advantage of aggregated opacity: low opacity of individual points means that overlap in dense areas has a stronger, more saturated color value.

TileMill不会像QGIS插件一样生成栅格化的热图。但是利用一些CartoCSS技巧，可以实现近似的效果。这些技巧包括聚合透明度：低透明度的点意味着在密集区域相互叠加会得到更加强化的和饱和的颜色值。

This example uses the original shapefile with just a few lines of CartoCSS:

对原始矢量数据shapefile使用如下几行CartoCSS即可得到所需的效果：

	
	#abralicenseept [DESCRIPTIO != 'Retailer B']{
	  marker-width:4;
	  marker-fill:#ef0;
	  marker-opacity:.45;
	  marker-line-opacity:0;
	  marker-allow-overlap:true;
	} 
	

_Note: This code also omits any locations listed as 'Retailer B' to get rid of grocery stores to better show where bars and nightlife are in DC._

_注意：这段代码忽略了所有’Retailer B’，也就是去掉了那些杂货店点。这样可以更好的反映出华盛顿特区中酒吧的位置，以及夜生活的分布情况。_

This approach has the added benefit of retaining the exact locations of individual dots at higher zoom levels, as well as the possibility of feature-specific interactivity. For further styling tips, learn more about advanced map design.

这种方法的好处在于能够在地图放大到较高级别时保留和展示每个点的精确信息，而且还可以支持要素级的交互。更多关于样式配置方面的内容，可以参见本章中“高级地图设计”一节。