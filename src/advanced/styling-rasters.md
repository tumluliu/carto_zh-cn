### 栅格制图技巧

_译注：[原文1](https://www.mapbox.com/tilemill/docs/guides/georeferencing-satellite-images/)，[原文2](https://www.mapbox.com/tilemill/docs/guides/colorizing-single-band-raster-data/)，[原文3](https://www.mapbox.com/tilemill/docs/guides/color-correction-rgb-imagery/)，[原文4](https://www.mapbox.com/tilemill/docs/guides/discrete-raster-data/)，[原文5](https://www.mapbox.com/tilemill/docs/guides/terrain-data/)_

#### 配准卫星影像（Georeferencing Satellite Images）

The skies were clear on President Obama’s first Inauguration and [GeoEye](http://geoeye.com/CorpSite/) was there to capture the incredible imagery from space.

奥观海总统首次就职典礼的那天，天空万里无云。那时[GeoEye](http://geoeye.com/CorpSite/)恰好在白宫上空，它从太空中捕捉到了那个精彩的瞬间：

![](http://farm9.staticflickr.com/8354/8380566083_f2e66936a9_o.jpg)
_GeoEye | 2009 Inauguration_

Using [GDAL](http://www.gdal.org/), [QGIS](http://www.qgis.org/), and [MapBox Satellite](http://mapbox.com/blog/mapbox-satellite/), we were able to manually georeference the 2009 imagery and compare the ceremony attendance and changes to the city with our own [MapBox Satellite layer](http://mapbox.com/blog/mapbox-satellite/).

利用[GDAL](http://www.gdal.org/)，[QGIS](http://www.qgis.org/)，还有[MapBox Satellite](http://mapbox.com/blog/mapbox-satellite/)等工具，我们可以对这幅2009年的卫星影像手工配准，然后通过与[MapBox Satellite layer](http://mapbox.com/blog/mapbox-satellite/)的对比来观察当时的就职典礼出席来宾，发现城市的变化。

Below, we go over the steps of how to take a non-georeferenced JPEG image and turn it into a geospatial dataset ready for rendering in [TileMill](http://mapbox.com/tilemill/) and uploading to MapBox hosting. We use three free, open-source software libraries: [Quantum GIS](http://hub.qgis.org/projects/quantum-gis/wiki/Download) (These instructions are based off of QGIS version 1.8.0 Lisboa), [GDAL](http://www.gdal.org/), and [TileMill](http://mapbox.com/tilemill/).

下面我们就来介绍如何将一幅缺少空间参考信息的普通JPEG图片转成一个可以在制图工具中渲染的地理空间数据集。整个过程中需要用到三个免费、开源的软件工具：[Quantum GIS](http://hub.qgis.org/projects/quantum-gis/wiki/Download)（这里使用的QGIS版本为1.8），[GDAL](http://www.gdal.org/)，还有[TileMill](http://mapbox.com/tilemill/)。

##### 手工配准（Manual Georeferencing）

1. Open up QGIS and activate the Georeferencer plugin from within the Plugins drop-down menu.  
	打开QGIS，从Plugins下拉菜单中激活Georeferencer插件。
2. Open raster dataset in Georeferencer window.  
	在Georeferencer窗口中打开栅格数据集。
3. Log in to your MapBox account and create a new map layer. To create a Satellite base layer, you’ll need a [basic account or higher](http://mapbox.com/plans/).  
	登录MapBox账户并创建一个新的图层。要使用MapBox的卫星影像图层，你至少需要一个[基本账户](http://mapbox.com/plans/)。
4. In the Georeferencer window in QGIS, choose a recognizable location on the source image. Select the **Add a Point** tool (Command + A), and add a point on the source image over the location. Here we are using the corner of 15th Street NW and Madison Dr. NW, across from the Washington Monument.  
	在QGIS的Georeferencer窗口中，从源图片中找一个明显的标志性位置。用**Add a Point**工具（Mac下的快捷键为Cmd+A）在刚才的位置上放一个点。这里我们选取的位置是15th Street NW和Madison Dr. NW的交叉口，在华盛顿纪念碑对面。
5. Search for the same location on the map shown in your custom MapBox Satellite layer, keeping the point of interest in the center of the screen. Here’s a screenshot of the area on the source image.  
	在你的MapBox卫星影像图层上找到和上一步中完全相同的位置，并且拖动地图使该点位于屏幕中心，如下图所示。

![](http://farm9.staticflickr.com/8046/8380963926_b3a0689e17_b.jpg)

![](http://farm9.staticflickr.com/8196/8383025229_89875305ce_b.jpg)
_MapBox Satellite_

Take a look at the url hash at the end of the MapBox url.

注意URL中最后的部分：

	#17.00/38.89030/-77.03294

The first number after the `#` is the **zoom level**, the second is **latitude**, and the third is **longitude**.  
  
在`#`之后的第一个数字是**缩放级别**，第二个是**纬度**，第三个是**经度**。

**Latitude**: 38.89030
**Longitude**: -77.03294

6. Back in QGIS, use the values obtained from the url hash in the “Enter Map Coordinates” dialog. The second number from the URL hash, **latitude**, is the **Y** value; the third number, **longitude**, from the hash is the **X** value.  
	再回到QGIS中，在“Enter Map Coordinates”对话框中输入从URL中获得的经纬度座标。注意**纬度**对应的是**Y**值，**经度**对应的是**X**值。

![](http://farm9.staticflickr.com/8193/8381021830_bd4d8d81e0_b.jpg)

7. Repeat steps 6-8 until you’ve added the desired number of control points to the image. A good rule of thumb here is to start with the four corners and work your way inward. To meet the project accuracy requirements, we added a total of 37 ground control points. Be sure to save your ground control points using the “**Save GCP Points as**” option in the georeferencer plugin. That way, you can reopen the project at a later date to modify points or add additional ones to improve spatial accuracy.  
	重复第6到第8步的操作，直到已经在原始图片上增加了足够多的控制点。推荐的做法是按照从图片的四个角开始，逐渐向图片中心推进的方式选取控制点。为了达到项目的精度需求，我们一共选取了37个控制点。然后请务必用Georeferencer插件中的“**Save GCP Points as**”功能保存这些控制点，这样你才可以在以后重新打开这些控制点，修改它们或添加新的控制点来进一步提高空间精确度。
8. You can either perform the georeferencing within QGIS, or select the “**Generate GDAL Script**” from QGIS. We selected Thin Plate Spline transformation, Lanczos resampling, no compression, and generated a script to modify before running.  
	配准操作既可以直接在QGIS中做，也可以通过选择“**Generate GDAL Script**”来生成GDAL脚本。我们这里选择了Thin Plate Spline变换、Lanczos重采样、无压缩选项之后，生成了一段可以修改的脚本。

![](http://farm9.staticflickr.com/8232/8381188430_f6e186bcb4_o.png)

Here’s our final processing script, which incorporates the QGIS-generated ground control points, and my project-specific projection, resampling, and overview settings.

下面就是最终的处理脚本。它是基于QGIS生成的脚本，结合了我们项目中的地图投影、重采样和缩略图等特定配置的版本。

	
	#!/bin/bash
	 
	ADDO="2 4 8 16 32 64 128 256 512 1024 2048 4096 8192"
	
	gdal_translate \
	  -of GTiff \
	  -a_nodata "0 0 0" \
	  -a_srs EPSG:4326 \
	  -gcp 726.415 736.655 -77.0501 38.9025 \
	  -gcp 7907.04 3607.98 -77.0091 38.8898 \
	  -gcp 5478.38 478.625 -77.0231 38.9037 \
	  -gcp 1725.89 7262.68 -77.0443 38.8731 \
	  -gcp 3094.56 1849.79 -77.0365 38.8975 \
	  -gcp 3038.65 3730.89 -77.0367 38.889 \
	  -gcp 8098.32 6969.63 -77.0076 38.8744 \
	  -gcp 6988.43 324.384 -77.0141 38.9044 \
	  -gcp 8066.53 1871.22 -77.0079 38.8974 \
	  -gcp 735.208 3692.09 -77.0501 38.8893 \
	  -gcp 166.054 6045.52 -77.0533 38.8786 \
	  -gcp 7344.64 7467.87 -77.012 38.8722 \
	  -gcp 4911.28 5353.86 -77.026 38.8817 \
	  -gcp 4621.91 6858.44 -77.0276 38.8749 \
	  -gcp 2528.87 5761.23 -77.0396 38.8798 \
	  -gcp 5429.01 3224.15 -77.0229 38.8913 \
	  -gcp 5433.9 3222.92 -77.0229 38.8913 \
	  -gcp 3698.56 3448.02 -77.0329 38.8903 \
	  -gcp 3623.56 3631.47 -77.0334 38.8894 \
	  -gcp 3009.31 3514.59 -77.0369 38.89 \
	  -gcp 3283.65 3610.7 -77.0353 38.8896 \
	  -gcp 2927.7 4022.01 -77.0373 38.8877 \
	  -gcp 3892.68 3804.94 -77.0318 38.8887 \
	  -gcp 3058.53 5374.32 -77.0366 38.8816 \
	  -gcp 4093.72 5910.69 -77.0306 38.8792 \
	  -gcp 7320.98 3316.66 -77.012 38.8909 \
	  -gcp 7738.56 3826.98 -77.0097 38.8887 \
	  -gcp 7739.92 3295.92 -77.0097 38.891 \
	  -gcp 7755.51 3482.34 -77.0097 38.8902 \
	  -gcp 7296.21 3723.98 -77.0122 38.889 \
	  -gcp 6804.33 3235.89 -77.015 38.8912 \
	  -gcp 6801.08 3869.32 -77.015 38.8884 \
	  -gcp 319.63 7391.94 -77.0524 38.8725 \
	  -gcp 300.338 3897.82 -77.0525 38.8883 \
	  -gcp 265.529 3509.08 -77.0527 38.89 \
	  -gcp 1038.55 3611.29 -77.0482 38.8895 \
	  -gcp 1039.54 3708.54 -77.0482 38.8891 \
	  Inauguration.jpg \
	  Inauguration_4326.tif
	gdalwarp \
	   -r lanczos \
	   -rcs \
	   -t_srs EPSG:3857 \
	   -wm 1000 \
	   -srcnodata "0 0 0" \
	   -dstnodata "0 0 0" \
	   -dstalpha  \
	   -co COMPRESS=LZW \
	   -co TILED=YES \
	   Inauguration_4326.tif \
	   Inauguration_3857.tif
	gdaladdo \
	   -r gauss \
	   --config COMPRESS_OVERVIEW LZW \
	   Inauguration_3857.tif \
	   $ADDO
	rm Inauguration_4326.tif
	

The script produces a conventional GeoTIFF, which we can render in [TileMill](http://mapbox.com/tilemill/), and upload to MapBox hosting.

执行上面这段脚本会生成一个可以在TileMill中渲染的标准GeoTIFF。

![](http://farm9.staticflickr.com/8502/8380218977_f45a5a7532_o.png)

We can check the spatial accuracy of the georeferencing against our [MapBox Satellite layer](https://www.mapbox.com/tilemill/docs/guides/georeferencing-satellite-images/mapbox.com/blog/mapbox-satellite/) using the Reference Layer Plugin from within TileMill.

我们可以在TileMill中利用Reference Layer插件将这幅配准后图像与[MapBox Satellite layer](https://www.mapbox.com/tilemill/docs/guides/georeferencing-satellite-images/mapbox.com/blog/mapbox-satellite/)对比来检查其空间精度。

#### 为单波段栅格数据着色（Colorizing Single-band Raster Data）

Single band raster data traditionally rendered as black and white in TileMill, but it’s no longer so black and white.

传统上，单波段的栅格数据在TileMill中会被渲染成黑白的，但它也完全可以不被绘制成黑白分明的样子。

![](http://farm9.staticflickr.com/8524/8518337247_8cbf2c48e3_o.png)

See our blog post on [processing DNB raster data from NASA and NOAA’s Suomi NPP spacecraft](http://mapbox.com/blog/nighttime-lights-nasa-noaa/) to create a nighttime lights map, showing lights visible from space at night. Thanks to raster-colorizer, we can now generate the same map with half as many lines of code, in a fraction of the time, by performing all of the false color steps from within TileMill, rather than a [combination of command line tools and virtual rasters (VRT)](https://gist.github.com/hrwgc/4694661).

MapBox的博文[processing DNB raster data from NASA and NOAA’s Suomi NPP spacecraft](http://mapbox.com/blog/nighttime-lights-nasa-noaa/)介绍了如何制作一幅夜间灯光地图，让我们领略到从太空中看地球上夜晚灯光分布的美妙效果。而现在在TileMill中，我们只需一半的代码，用更短的时间就可以制出同样效果的地图。这要归功于强大的`raster-colorizer`。有了它，我们就不用再费劲的用[命令行工具加虚拟栅格（VRT）](https://gist.github.com/hrwgc/4694661)的方法了。

To take advantage of the raster-colorizer functionality in TileMill, be sure to set `band=1` in the Advanced input area of TileMill’s “Add Layer” window.

要充分利用强大的`raster-colorizer`，请先确认在图层的高级设置中加入了`band=1`。（译注：其实我觉得这很不自然，在使用HiGIS的制图前端过程中，大家就经常会忘记加这个东西。这更像是个神秘的trick，应该在今后的设计中修正）

##### 暗夜的灯光（Lights of the Night）

The source data is one band, but we can render it in TileMill as a three band raster using CartoCSS classes, resulting in three versions of the layer rendering on top of one another:

尽管原始数据只有一个波段，但我们可以利用CartoCSS的从属样式能力把它像一幅三波段数据那样去渲染，得到同一个图层的三个版本，相互叠加渲染：

	
	#2010::1 #2010::2 #2010::3
	

Next, use the `raster-colorizer` functionality to color each class a different color to achieve the desired RGB finished product:

下一步，利用`raster-colorizer`对每个从属样式分别定义不同的渲染色带，从而最终得到合成的RGB效果：

![](http://farm9.staticflickr.com/8386/8518337209_51e27be3a5_z.jpg)

![](http://farm9.staticflickr.com/8507/8519450546_d6c5299ef4_o.png)

_2010 NightTime Lights_

	
	#2010::1  {
	  raster-scaling:gaussian;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.41;
	  raster-colorizer-stops:
	    stop(0,transparent,linear)
	    stop(80,#fff)
	    stop(100,#000)
	}
	#2010::2  {
	  raster-scaling:gaussian;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.41;
	  raster-colorizer-stops:
	    stop(0,transparent,linear)
	    stop(50,#ffcc00)
	    stop(60,#000)
	}
	#2010::3  {
	  raster-scaling:gaussian;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.41;
	  raster-colorizer-stops:
	    stop(0,transparent,linear)
	    stop(90,#fa360b)
	    stop(120,#000)
	}
	

###### 最终效果图（Finished Map）

![](http://farm9.staticflickr.com/8107/8519450576_a2e35a1404_o.jpg)

#### 全色影像的色彩校正（Color Correction of RGB Imagery）

Following a similar process to Single-band colorizing, we can perform color correction for 3-band natural-color RGB aerial or satellite imagery from within TileMill. Performing the color modifications from within TileMill is much easier and offers greater customization than my previous methods offered.

与前面介绍的单波段栅格数据着色过程类似，我们可以对三波段可见光航空或卫星影像进行色彩校正。利用CartoCSS和相关工具（例如TileMill）可以使这项工作大为简化，而且可定制性更强。

##### 全色影像（RGB Imagery）
 
Normally, when you load an RGB image as a layer in TileMill the layer displays as natural color. To take advantage of the `raster-colorizer` functionality, we need to add the layer three times, including in the advanced option of `band=1` for the red layer, `band=2` for the green layer, and `band=3` for the blue layer. The `band=` advanced option has TileMill load only the indicated band.

正常情况下，一幅全色影像在加载到CartoCSS制图工具中之后会以可见光自然色显示。但为了能充分利用`raster-colorizer`，我们需要对这同一幅全色影像加载三次，而且三次对应的红、绿、蓝图层要分别在高级设置中加上`band=1`、`band=2`和`band=3`选项。这里`band=`的含义是告诉制图工具只加载指定波段的数据。

To turn the three layers back into an RGB image, you’ll want to use `raster-comp-op: plus;` and `raster-colorizer-default-mode: linear;` for each layer.

为了能让这三个图层叠加后仍能按照全色影像正常显示，还需要为每个图层定义`raster-comp-op: plus;`和`raster-colorizer-default-mode: linear;`属性。

![](http://farm9.staticflickr.com/8379/8496556690_54a513891e_o.png)
_色彩校正前_

	
	#red {
	  raster-scaling:gaussian;
	  raster-comp-op:plus;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.1;
	  raster-colorizer-stops:
	    stop(0,#000)
	    stop(255,rgb(255,0,0))
	}
	#green {
	  raster-scaling:gaussian;
	  raster-comp-op:plus;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.1;
	  raster-colorizer-stops:
	    stop(0,#000)
	    stop(255,rgb(0,255,0))
	}
	#blue {
	  raster-scaling:gaussian;
	  raster-comp-op:plus;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.1;
	  raster-colorizer-stops:
	    stop(0,#000)
	    stop(255,rgb(0,0,255))
	}
	

With the layer rendering as an RGB image in TileMill, you can now apply color corrections to each band, simply by modifying the `raster-colorizer-stops`.

现在就可以利用`raster-colorizer-stops`对每个波段对应的图层进行色彩校正了。

A good starting place for color correcting in this manner is to adjust the min, max, and mean values. We found red and green bands looked best when we set the minimum to 20 and maximum to 200; for the blue band I set the minimum value to 40. For the red layer, pixels with values less than or equal to 20 are all registered as the darkest dark elements of the band, and all pixels with values greater than or equal to 200 are registered as the brightest red.

色彩校正可以从调整最大、最小和均值开始。我们发现红色和绿色波段在将最小值设为20、最大值设为200，蓝色波段的最小值设为40时具有最好的视觉效果。对于红色波段图层，所有像素值小于等于20的都会被置为最暗的深色，而所有大于等于200的像素都会被置为最亮的红色。

![](http://farm9.staticflickr.com/8248/8518337589_8552d1e37b_z.jpg)

![](http://farm9.staticflickr.com/8368/8495452361_4462b93770_o.png)
_色彩校正后_

	
	#blue {
	  raster-scaling:gaussian;
	  raster-comp-op:plus;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.1;
	  raster-colorizer-stops:
	    stop(20,#000)
	    stop(200,rgb(0,0,255))
	}
	#green {
	  raster-scaling:gaussian;
	  raster-comp-op:plus;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.1;
	  raster-colorizer-stops:
	    stop(20,#000)
	    stop(200,rgb(0,255,0))
	}
	#red {
	  raster-scaling:gaussian;
	  raster-comp-op:plus;
	  raster-colorizer-default-mode:linear;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.1;
	  raster-colorizer-stops:
	    stop(40,#000)
	    stop(200,rgb(255,0,0))
	}
	

#### 离散栅格数据（Discrete Raster Data）

##### 离散栅格数据：土地覆盖（Discrete Raster Data: Land Cover）

Contextually styling a discrete raster data set – a task once completed over several steps across different applications – can be completed within TileMill, our open source design studio. When you contextually style raster data, you bind a color value to particular pixel values, which is great for highlighting urban areas using a bright color, making no-data pixels appear transparent, and grouping similar categories, like types of tree cover, into larger categories and making them all green.

基于上下文的离散栅格数据的制图通常需要多个软件工具相互配合，且经过多个步骤才能完成。而使用CartoCSS制图工具，这项工作则可以一站式搞定。在进行基于上下文的栅格制图时，具有特定值的像素会与某种颜色绑定。这在利用特别的颜色突出显示某些部分的场合下非常有用，例如用亮色表示城镇区域、让no-data值全部透明、将相同类别的地块（比如同一种植被覆盖的区域）合并使用一种颜色渲染等。

Here we will make a custom land cover map layer from a raster dataset. This guide uses [this one available from the Japan Aerospace Exploration Agency](http://www.eorc.jaxa.jp/ALOS/lulc/lulc_jindex.htm). Here is a [direct link to the zip file](http://www.eorc.jaxa.jp/ALOS/lulc/data/ver1302_LC_GeoTiff.tar.gz).

这里我们就来基于栅格数据制作一幅土地覆盖图。原始数据来源于[日本空间局](http://www.eorc.jaxa.jp/ALOS/lulc/lulc_jindex.htm)，zip文件的下载地址在[这里](http://www.eorc.jaxa.jp/ALOS/lulc/lulc_jindex.htm)。

The only pre-processing required is to reproject the dataset to Google Mercator projection, using an application like `gdalwarp`. All styling of the raster data can be accomplished from within TileMill using CartoCSS. 

预处理阶段唯一需要做的就是将该数据用`gdalwarp`重投影成Google Mercator投影。后面的制图样式配置全部可以用CartoCSS制图工具完成。

##### 预处理（Pre-processing）

After downloading and uncompressing the GeoTiff data, warp each image to the proper projection as we did to the [Natural Earth GeoTiff](http://www.mapbox.com/tilemill/docs/guides/reprojecting-geotiff/#reproject_and_add_a_geotiff_raster).

在下载并解压得到GeoTIFF数据之后，需要将其重投影，方法可以参考[这里](http://www.mapbox.com/tilemill/docs/guides/reprojecting-geotiff/#reproject_and_add_a_geotiff_raster)。

Warp all the images and move the reprojected ones to a directory called target (using Terminal):

下面这段bash脚本就是将所有位于target目录中的影像数据重投影：

	
	ls *.tif > abc
	mkdir target
	while read line
	do
	file=$(echo $line |awk -F. '{ print $1 }')
	gdalwarp -t_srs EPSG:3857 $line target/$file.tif
	done < abc
	

##### 构建虚拟数据集（Build a Virtual Dataset）

Since TileMill natively supports [GDAL’s Virtual Raster (VRT) format](http://www.gdal.org/gdal_vrttut.html), we can take advantage of a VRT rather than creating a new GeoTIFF mosaic. [`gdalbuildvrt`](http://www.gdal.org/gdalbuildvrt.html) creates a single XML file from the source images that is read as a single mosaic image in TileMill. **Make sure you use absolute paths for the source images when you use `gdalbuildvrt`**.

很多CartoCSS制图工具（例如TileMill）原生支持[GDAL虚拟栅格格式](http://www.gdal.org/gdal_vrttut.html)。因此我们就可以充分利用这一特性，而不需要将所有的影像重新拼接成一个新的GeoTIFF。使用[`gdalbuildvrt`](http://www.gdal.org/gdalbuildvrt.html)工具可以基于原始影像数据集生成一个XML文件，然后它就可以被CartoCSS制图工具识别成一个拼接好的影像了。**请注意在用`gdalbuildvrt`处理原始影像时要使用绝对路径**。

	$ gdalbuildvrt mosaic.vrt /absolute/path/to/input/tiffs/*.tif

##### 在CartoCSS制图工具中配置样式（Importing and Styling in TileMill）

While in the TileMill “Add Layer” window, input `band=1` in the Advanced input area. If you omit this step, the colorizer will not function properly.

在添加图层的时候，注意要加上`band=1`，否则着色器会工作不正常。

Since we’re using a land cover GeoTIFF with specific pixel values mapping directly to land cover classifications we want to use `raster-colorizer-default-mode: exact` meaning stops will map to exact pixel values, and no other color values will be assigned through interpolation.

由于我们使用的土地覆盖GeoTIFF数据中的每个像素都直接对应某种地块分类，所以需要使用`raster-colorizer-default-mode: exact`属性来指明每个颜色值都与特定的像素值一一对应，而在相邻的颜色值之间不需要插值填充。

Now all that remains is to translate the land use data key into CartoCSS style rules, using the `raster-colorizer` stop syntax:

剩下要做的就是把土地利用数据中的各种像素值与不同的颜色对应。这在`raster-colorizer`中应该采用以下语法：

`stop(` + pixel value + `,` + color to assign + `)`

![](http://farm9.staticflickr.com/8385/8495388263_1a2c4eceb4_o.png)

	
	@blank:            transparent;
	@snow:             #ffffff;
	@unused:           #9a9a9a;
	@urban:            #e2bf58;
	@agriculture:      #91a487;
	@grass:            #6b7e60;
	@forest:           #46533f;
	@water:            #37526d;
	Map { background-color:@water;}
	
	#japan {
	  raster-opacity:1;
	  raster-scaling:gaussian;
	  raster-colorizer-default-mode:exact;
	  raster-colorizer-default-color: transparent;
	  raster-colorizer-epsilon:0.41;
	  raster-colorizer-stops:
	    stop(0, transparent)
	    stop(1, @water)
	    stop(2, @urban)
	    stop(3, @agriculture)
	    stop(4, @agriculture)
	    stop(5, @grass)
	    stop(6, @forest)
	    stop(7, @forest)
	    stop(8, @forest)
	    stop(9, @forest)
	    stop(10, @unused)
	    stop(11, @snow)
	    stop(253, @unused)
	    stop(255, @blank);
	}
	

###### 最终效果图（Finished Map）

![](http://farm9.staticflickr.com/8094/8495387917_8425ce6b97_o.jpg)

#### 地形数据制图（Working with terrain data）

To get good-looking terrain maps, we usually combine several types of visualizations generated by the [GDAL](https://www.mapbox.com/tilemill/docs/guides/gdal) DEM utilities.

为了得到赏心悦目的地形图，我们通常需要利用[GDAL](https://www.mapbox.com/tilemill/docs/guides/gdal)中的DEM工具生成若干中不同类型的可视化效果，然后再将其合理组合。

##### DEM数据源（Digital Elevation Model sources）

There are many different data formats for storing and working with digital elevation models. In our examples we’ll be working with geotiffs. Here are some examples of high quality free datasets that are available in geotiff format:

用于存储数字高程模型的数据格式有很多种。在我们的例子中将使用GeoTIFF。下面列出几个提供高质量免费GeoTIFF格式地形数据的数据源：

###### SRTM

Data collected from NASA’s [Shuttle Radar Topography Mission](http://www2.jpl.nasa.gov/srtm/) is a high quality source of elevation data covering much of the globe. It is available free from NASA directly, however we recommend working with [CGIAR’s cleaned up version](http://srtm.csi.cgiar.org/), which is also free.

数据来源于NASA的[Shuttle Radar Topography Mission](http://www2.jpl.nasa.gov/srtm/)。它是一个高程数据的高质量数据源，数据覆盖了地球表面的绝大多数地区。尽管SRTM数据可以从NASA直接免费下载，但我们还是推荐使用[CGIAR的清理后版本](http://srtm.csi.cgiar.org/)，它也同样是免费的。

###### ASTER

Aster is another global DEM datasource. It has better coverage of the earth’s surface than SRTM, and is slightly higher-resolution, but contains more errors than CGIAR’s clean SRTM set. Errors are usually in the form of spikes or pits, and can be significant.

Aster是另一个全球DEM数据源。与SRTM相比，Aster对地球表面的覆盖率更高，而且分辨率也更高一些。但Aster数据中的误差比CGIAR清理后的SRTM数据多。这些误差通常是一些尖峰或凹坑，而且还比较明显。

###### USGS NED

The US Geological Survey publishes a [National Elevation Dataset](http://ned.usgs.gov/) for the United States. It high resolution and frequently updated from a variety of sources.

美国地质调查局也发布了一个覆盖美国的[国家高程数据集（National Elevation Dataset）](http://ned.usgs.gov/)。它的分辨率高，而且从多个来源频繁更新。

##### 地形数据可视化的类型（Types of Visualizations）

###### 彩色地形图（也叫分层设色）（Color-relief (or hypsometric tint)）

![](https://www.mapbox.com/tilemill/assets/pages/relief-example.png)

Color relief assigns a color to a pixel based on the elevation of that pixel. The result is not intended to be physically accurate, and even if natural-looking colors are chosen the results should not be interpreted as representative of actual landcover. Low-lying areas are often given assigned green and yellow shades, with higher elevations blending to shades of grey, white, and/or red.

彩色地形图是根据每个像素的高程值赋予其一个颜色。其效果并不追求物理上的精确，而且即使是选择了自然光效果的颜色，也不应该将其与地块类型联系起来（译注：就是说渲染成绿色的地区不是指的那里就是森林绿地）。地势低洼的地区通常使用绿色和黄色来渲染，而在海拔较高的地区则倾向于使用灰色、白色和/或红色来渲染。

###### 山体阴影图（也叫地形阴影图）（Hillshade (or shaded relief)）

Hillshade visualization analyzes the digital elevation model to simulate a 3-dimensional terrain. The effect of sunlight on topography is not necessarily accurate but gives a good approximation of the terrain.

山体阴影图是将数字高程模型进行分析并模拟出三维地形的一种可视化效果图。阳光照射山体产生阴影的效果不要求精确，但提供了对真实地形可视化效果的一种良好近似。

###### 坡度图（Slope）

Slope visualization assigns a color to a pixel based on the difference in elevation between it and the pixels around it. We use it to make steep hills stand out and enhance the look of our terrain.

坡度图基于每个像素与其周围像素的高程差来为该像素赋予一个颜色。使用坡度图可以让陡峭的山体在地图上更加明显，从而增强地形起伏的可视化效果。

##### 使用GDAL-DEM实现地形可视化（Visualizations with GDAL-DEM）

###### 对数据重投影（Reprojecting the Data）

Chances are your DEM datasource will not come in the Google Mercator projection we need - for example, SRTM comes as [WGS84](http://spatialreference.org/ref/epsg/4326/) (EPSG:4326) and USGS NED comes as [NAD83](http://spatialreference.org/ref/epsg/4269/) (EPSG:4269).

原始的DEM数据通常都不是Google Mercator投影。例如SRTM的参考系是[WGS84](http://spatialreference.org/ref/epsg/4326/)（EPSG:4326），而USGS NED是[NAD83](http://spatialreference.org/ref/epsg/4269/)（EPSG:4269），所以它们在使用之前都需要重投影。

For our example we’ll be working with some NED data of the District of Columbia area. The following command will reproject the file to the proper projection - see [Reprojecting a GeoTIFF for TileMill](https://www.mapbox.com/tilemill/docs/guides/reprojecting-geotiff/) for more detailed information.

例如我们需要用哥伦比亚大区的NED数据来作图，那么需要使用以下命令对其重投影（参见前面关于栅格数据重投影的介绍）。

	
	gdalwarp -s_srs EPSG:4269 -t_srs EPSG:3785 -r bilinear dc.tif dc-3785.tif
	

In the case of reprojecting elevation data, the `-r bilinear` option is important because other resampling methods tend to produce odd stripes or grids in the resulting image.

在对高程数据重投影的时候，`-r bilinear`参数非常重要。因为其它的重采样方法会导致结果图像中产生奇怪的条纹或网格。

###### 制作山体阴影图（Creating hillshades）

Run this command to generate the shaded relief image:

执行以下命令生成地形阴影图：

	
	gdaldem hillshade -co compress=lzw dc-3785.tif dc-hillshade-3785.tif
	

The `-co compress=lzw` option will compress the TIFF. If you’re not concerned about disk space you can leave that part out. If you are using GDAL 1.8.0 or later you may also want to add the option `-compute_edges` in order to avoid a black pixel border around the edge of the image.

`-co compress=lzw`参数将对TIFF数据进行压缩。如果你的磁盘空间足够大，那么就可以不加压缩参数。如果你使用的是1.8.0以上版本的GDAL（译注：翻译本文档时GDAL的最新版本为1.11），那么还可以加上`-compute_edges`参数以防止地形数据的周围被填充黑色像素值。

Our output looks like this:

输出结果如下图：

![](https://www.mapbox.com/tilemill/assets/pages/hillshade-dc.png)

**Note:** If there is not a 1:1 relation between your vertical and horizontal units, you will want to use the-s (scale) option to indicate the difference. Since Google Mercator X & Y units are meters, and NED elevation is also stored in meters this wasn’t necessary for our example. If your elevation data is stored in feet, you might do this instead (since there are approximately 3.28 feet in 1 meter):

**注意：**如果数据的水平和垂直方向的度量单位比例关系不是1:1，那么需要加上`-s`（即scale，比例）参数来指定其差别。由于在Google Mercator投影中X和Y方向都是以米为单位，而NED数据也是以米为单位的，所以不需要加这个参数。但如果你的高程数据是以英尺为单位存储的，那么就需要加这个参数了（因为它们的换算关系大约是1米等于3.28英尺）：

	
	gdaldem hillshade -s 3.28 -co compress=lzw dc-3785.tif dc-hillshade-3785.tif
	

###### 制作彩色地形图（Generating color-relief）

Before you can create a color-relief image, you need to decide what colors you want to assign to different elevations. This color configuration is created and stored in a specially formatted plain text file. Each line in the file should have four numbers - an elevation, and an RGB triplet of numbers from 0 to 255. To figure out the proper RGB values of a color, you can used the color selection tool of any image editor, or an online too such as [ColorPicker.com](http://www.colorpicker.com/).

在制作彩色地形图之前，你需要先想好对于不同的海拔高度应该赋予什么颜色。这个色彩配置将被存储在一个具有特定格式的文本文件中。文件中的每一行都是四个数字：先是高程值，然后是RGB的三个分量（取值范围都是0到255）。为了得到合理的RGB颜色，你可以使用任意一个图像编辑工具中的调色板，或者利用像[ColorPicker.com](http://www.colorpicker.com/)这样的在线工具。

Here is our example, saved to a file called ramp.txt:

我们这里给出一个例子，保存在名为ramp.txt的文件中：

	
	0 46 154 88
	1800 251 255 128
	2800 224 108 31
	3500 200 55 55
	4000 215 244 244
	

The above numbers define a gradient that will blend 5 colors over 4000 units of elevation. (Our units are meters, but we don’t need to specify that in the file.) They will translate to a color-to-elevation relationship that looks like this:

上面的色彩配置定义了一组包含5个颜色的色阶，涵盖了高度落差为4000个度量单位的区间（我们这个数据集中是以米为单位的，但在这个文件中不需要明确具体的单位）。这个色彩配置会被解译成一个颜色-高程对应关系的色带：

![](https://www.mapbox.com/tilemill/assets/pages/ramp-illustration-fs8.png)

To apply this color ramp to the DEM, use the `gdaldem color-relief` command:

要在DEM数据上应用这个色带，需要使用`gdaldem color-relief`命令：

	
	gdaldem color-relief input-dem.tif ramp.txt output-color-relief.tif
	

The area you are working with may not have such a wide range of elevations. You can check the minimum & maximum values of your DEM to adjust your ramp style accordingly. The `gdalinfo` command can tell us the range of values in the geotiff:

但在你选定要制图的目标区域中，未必会有这么大跨度的高程落差。你可以根据目标区域中地形的海拔最高与最低值来调整色彩配置。利用`gdalinfo`命令可以帮助你看到GeoTIFF中的高程值的统计信息：

	
	gdalinfo -stats your_file.tif
	

Among other things, this will tell you the minimum, maximum, and mean values of your elevation data to help you make decisions about how to choose your colors. With elevations tweaked for our area around DC, this is what we get:

抛开其它信息，这个命令可以告诉你的高程数据中的最大值、最小值和均值，从而辅助你设计色彩配置表。在调整了色彩配置后，华盛顿特区周边地区的彩色地形图如下：

![](https://www.mapbox.com/tilemill/assets/pages/color-dc.png)

###### 制作坡度阴影图（Generating slope shading）

With the gdaldem tools, generating a slope visualization is a two step process.

利用gdaldem工具，制作坡度可视化效果包含两个步骤。

First, we generate a slope tif where each pixel value is a degree, from 0 to 90, representing the slope of a given piece of land.

首先，我们生成一个坡度tif，其中每个像素为一个0到90之间的角度值，表示对应区域的坡度。

	
	gdaldem slope dc-3785.tif dc-slope-3785.tif
	

_The same note about horizontal and vertical scale as hillshades applies for slope._

_这里也需要考虑水平和垂直方向的度量单位比例，和制作山体阴影图时一样。_

The tif output by `gdaldem slope` has no color values assigned to pixels, but that can be achieved by feeding it through gdaldem color-relief with an appropriate ramp file.

`gdaldem slope`生成的tif是没有为其中的像素值赋予颜色的，但可以通过gdaldem color-relief工具和色彩配置文件为其上色。

Create a file called ‘slope-ramp.txt’ containing these two lines:

新建一个名为slope-ramp.txt的文件，将以下两行拷进去并保存：

	
	0 255 255 255
	90 0 0 0
	

This file can then be referenced by by the color-relief command to display white where there is a slope of 0° (ie, flat) and display black where there is a 90° cliff (with angles in-between being various shades of grey). The command to do this is:

然后在调用color-relief命令时应用该色彩配置文件，即可得到坡度为0°（即完全平坦）的地方被渲染为白色，90°直上直下的峭壁处被渲染成黑色，而中间的其它值则被渲染成各级灰度。使用如下命令实现该效果：

	
	gdaldem color-relief -co compress=lzw dc-slope-3785.tif slope-ramp.txt dc-slopeshade-3785.tif
	

Which gives us:

然后得到的结果如下图所示：

![](https://www.mapbox.com/tilemill/assets/pages/dc-slope.png)

##### 合并最终结果（Combining the final result in TileMill）

To combine these geotiffs in TileMill, we’ll make use of the ‘multiply’ image blending mode using the `raster-comp-op` property, which stands for “compositing operation”. Assuming you have added your three geotiffs as layers with appropriate IDs, the following style will work. You can adjust the opacity of the hillshade and slope layers to tweak the style.

为了将最终结果合并，我们需要利用`raster-comp-op`合成操作中的‘multiply’混色模式。假设你已经将前面制作完成的三个GeoTIFF数据集加入了制图工具，得到三个图层，并分别赋予了合适的图层ID，那么就可以应用下面的样式配图了。你可以根据需要调整山体阴影和坡度图层的透明度以达到最佳效果。

	
	#color-relief,
	#slope-shade,
	#hill-shade {
	    raster-scaling: bilinear;
	    // note: in TileMill 0.9.x and earlier this is called raster-mode
	    raster-comp-op: multiply;
	}
	
	#hill-shade { raster-opacity: 0.6; }
	
	#slope-shade { raster-opacity: 0.4; }
	

The end result is something like this, ready to be combined with your vector data:

最终的结果如下图所示，它还可以再与其它矢量图层进一步叠加。

![](https://www.mapbox.com/tilemill/assets/pages/combined-dc.png)

##### 关于性能（Raster performance）

If your rasters are many MBs in size, then an important optimization is to build image pyramids, or overviews. This can be done with the `gdaladdo` tool or in QGIS. Building overviews does not impact the resolution of the original image, but it rather inserts new reduced resolution image references inside the original file that will be used in place of the full resolution data at low zoom levels. Any processing of the original image will likely drop any internal overviews previously built, so make sure to rebuild them if needed. You can use the gdalinfo tool to check if your tiff has overviews by ensuring the output has the ‘Overviews’ keyword reported for each band.

如果你的栅格数据有很多MB那么大（译注：这应该不算大吧，几个GB的可能还算），那么一个重要的优化手段就是构建影像金字塔，或缩略图。这可以利用`gdaladdo`工具或在QGIS来完成。构建缩略图不会影响原始影像的分辨率，它只是在原始文件中增加一系列分辨率递减的影像，从而在缩放级别较低时不必加载数据量较大的原始影像。然而如果对原始影像进行了编辑，那么之前构建的金字塔和缩略图都将会失效，需要重新构建。你可以利用gdalinfo工具检查你的tif文件中是否已经包含了金字塔/缩略图。如果在输出的关于每个波段的信息中有’Overviews’关键字，那么就说明已经构建了金字塔/缩略图。


