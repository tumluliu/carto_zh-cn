### 栅格制图技巧

_译注：[原文1](https://www.mapbox.com/tilemill/docs/guides/georeferencing-satellite-images/)，[原文2](https://www.mapbox.com/tilemill/docs/guides/colorizing-single-band-raster-data/)，[原文3](https://www.mapbox.com/tilemill/docs/guides/color-correction-rgb-imagery/)，[原文4](https://www.mapbox.com/tilemill/docs/guides/discrete-raster-data/)，[原文5](https://www.mapbox.com/tilemill/docs/guides/terrain-data/)_

#### 配准卫星影像（Georeferencing Satellite Images）

The skies were clear on President Obama’s first Inauguration and [GeoEye](http://geoeye.com/CorpSite/) was there to capture the incredible imagery from space.

![](http://farm9.staticflickr.com/8354/8380566083_f2e66936a9_o.jpg)
_GeoEye | 2009 Inauguration_

Using [GDAL](http://www.gdal.org/), [QGIS](http://www.qgis.org/), and [MapBox Satellite](http://mapbox.com/blog/mapbox-satellite/), we were able to manually georeference the 2009 imagery and compare the ceremony attendance and changes to the city with our own [MapBox Satellite layer](http://mapbox.com/blog/mapbox-satellite/).

Below, we go over the steps of how to take a non-georeferenced JPEG image and turn it into a geospatial dataset ready for rendering in [TileMill](http://mapbox.com/tilemill/) and uploading to MapBox hosting. We use three free, open-source software libraries: [Quantum GIS](http://hub.qgis.org/projects/quantum-gis/wiki/Download) (These instructions are based off of QGIS version 1.8.0 Lisboa), [GDAL](http://www.gdal.org/), and [TileMill](http://mapbox.com/tilemill/).

##### 手工配准（Manual Georeferencing）

1. Open up QGIS and activate the Georeferencer plugin from within the Plugins drop-down menu.
2. Open raster dataset in Georeferencer window.
3. Log in to your MapBox account and create a new map layer. To create a Satellite base layer, you’ll need a [basic account or higher](http://mapbox.com/plans/).
4. In the Georeferencer window in QGIS, choose a recognizable location on the source image. Select the **Add a Point** tool (Command + A), and add a point on the source image over the location. Here we are using the corner of 15th Street NW and Madison Dr. NW, across from the Washington Monument.
5. Search for the same location on the map shown in your custom MapBox Satellite layer, keeping the point of interest in the center of the screen. Here’s a screenshot of the area on the source image.

![](http://farm9.staticflickr.com/8046/8380963926_b3a0689e17_b.jpg)

![](http://farm9.staticflickr.com/8196/8383025229_89875305ce_b.jpg)
_MapBox Satellite_

Take a look at the url hash at the end of the MapBox url.

	#17.00/38.89030/-77.03294

The first number after the `#` is the **zoom level**, the second is **latitude**, and the third is **longitude**.

**Latitude**: 38.89030
**Longitude**: -77.03294

6. Back in QGIS, use the values obtained from the url hash in the “Enter Map Coordinates” dialog. The second number from the URL hash, **latitude**, is the **Y** value; the third number, **longitude**, from the hash is the **X** value.

![](http://farm9.staticflickr.com/8193/8381021830_bd4d8d81e0_b.jpg)

7. Repeat steps 6-8 until you’ve added the desired number of control points to the image. A good rule of thumb here is to start with the four corners and work your way inward. To meet the project accuracy requirements, we added a total of 37 ground control points. Be sure to save your ground control points using the “**Save GCP Points as**” option in the georeferencer plugin. That way, you can reopen the project at a later date to modify points or add additional ones to improve spatial accuracy.
8. You can either perform the georeferencing within QGIS, or select the “**Generate GDAL Script**” from QGIS. We selected Thin Plate Spline transformation, Lanczos resampling, no compression, and generated a script to modify before running.

![](http://farm9.staticflickr.com/8232/8381188430_f6e186bcb4_o.png)

Here’s our final processing script, which incorporates the QGIS-generated ground control points, and my project-specific projection, resampling, and overview settings.

	
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

![](http://farm9.staticflickr.com/8502/8380218977_f45a5a7532_o.png)

We can check the spatial accuracy of the georeferencing against our [MapBox Satellite layer](https://www.mapbox.com/tilemill/docs/guides/georeferencing-satellite-images/mapbox.com/blog/mapbox-satellite/) using the Reference Layer Plugin from within TileMill.

#### 为单波段栅格数据着色（Colorizing Single-band Raster Data）

Single band raster data traditionally rendered as black and white in TileMill, but it’s no longer so black and white.

![](http://farm9.staticflickr.com/8524/8518337247_8cbf2c48e3_o.png)

See our blog post on [processing DNB raster data from NASA and NOAA’s Suomi NPP spacecraft](http://mapbox.com/blog/nighttime-lights-nasa-noaa/) to create a nighttime lights map, showing lights visible from space at night. Thanks to raster-colorizer, we can now generate the same map with half as many lines of code, in a fraction of the time, by performing all of the false color steps from within TileMill, rather than a [combination of command line tools and virtual rasters (VRT)](https://gist.github.com/hrwgc/4694661).

To take advantage of the raster-colorizer functionality in TileMill, be sure to set `band=1` in the Advanced input area of TileMill’s “Add Layer” window.

##### Lights of the Night

The source data is one band, but we can render it in TileMill as a three band raster using CartoCSS classes, resulting in three versions of the layer rendering on top of one another:

`#2010::1 #2010::2 #2010::3`

Next, use the `raster-colorizer` functionality to color each class a different color to achieve the desired RGB finished product:

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
	

###### Finished Map

![](http://farm9.staticflickr.com/8107/8519450576_a2e35a1404_o.jpg)

#### 全色影像的色彩校正（Color Correction of RGB Imagery）

Following a similar process to Single-band colorizing, we can perform color correction for 3-band natural-color RGB aerial or satellite imagery from within TileMill. Performing the color modifications from within TileMill is much easier and offers greater customization than my previous methods offered.

##### 全色影像（RGB Imagery）
 
Normally, when you load an RGB image as a layer in TileMill the layer displays as natural color. To take advantage of the `raster-colorizer` functionality, we need to add the layer three times, including in the advanced option of `band=1` for the red layer, `band=2` for the green layer, and `band=3` for the blue layer. The `band=` advanced option has TileMill load only the indicated band.

To turn the three layers back into an RGB image, you’ll want to use `raster-comp-op: plus;` and `raster-colorizer-default-mode: linear;` for each layer.

![](http://farm9.staticflickr.com/8379/8496556690_54a513891e_o.png)
_Before color correction_

	
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

A good starting place for color correcting in this manner is to adjust the min, max, and mean values. We found red and green bands looked best when we set the minimum to 20 and maximum to 200; for the blue band I set the minimum value to 40. For the red layer, pixels with values less than or equal to 20 are all registered as the darkest dark elements of the band, and all pixels with values greater than or equal to 200 are registered as the brightest red.

![](http://farm9.staticflickr.com/8248/8518337589_8552d1e37b_z.jpg)

![](http://farm9.staticflickr.com/8368/8495452361_4462b93770_o.png)
_After color correction_

	
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

##### Discrete Raster Data: Land Cover

Contextually styling a discrete raster data set – a task once completed over several steps across different applications – can be completed within TileMill, our open source design studio. When you contextually style raster data, you bind a color value to particular pixel values, which is great for highlighting urban areas using a bright color, making no-data pixels appear transparent, and grouping similar categories, like types of tree cover, into larger categories and making them all green.

Here we will make a custom land cover map layer from a raster dataset. This guide uses [this one available from the Japan Aerospace Exploration Agency](http://www.eorc.jaxa.jp/ALOS/lulc/lulc_jindex.htm). Here is a [direct link to the zip file](http://www.eorc.jaxa.jp/ALOS/lulc/data/ver1302_LC_GeoTiff.tar.gz).

The only pre-processing required is to reproject the dataset to Google Mercator projection, using an application like `gdalwarp`. All styling of the raster data can be accomplished from within TileMill using CartoCSS. 

##### Pre-processing

After downloading and uncompressing the GeoTiff data, warp each image to the proper projection as we did to the [Natural Earth GeoTiff](http://www.mapbox.com/tilemill/docs/guides/reprojecting-geotiff/#reproject_and_add_a_geotiff_raster).

Warp all the images and move the reprojected ones to a directory called target (using Terminal):

	
	ls *.tif > abc
	mkdir target
	while read line
	do
	file=$(echo $line |awk -F. '{ print $1 }')
	gdalwarp -t_srs EPSG:3857 $line target/$file.tif
	done < abc
	

##### Build a Virtual Dataset

Since TileMill natively supports [GDAL’s Virtual Raster (VRT) format](http://www.gdal.org/gdal_vrttut.html), we can take advantage of a VRT rather than creating a new GeoTIFF mosaic. [`gdalbuildvrt`](http://www.gdal.org/gdalbuildvrt.html) creates a single XML file from the source images that is read as a single mosaic image in TileMill. **Make sure you use absolute paths for the source images when you use `gdalbuildvrt`**.

	$ gdalbuildvrt mosaic.vrt /absolute/path/to/input/tiffs/*.tif

##### Importing and Styling in TileMill

While in the TileMill “Add Layer” window, input band=1 in the Advanced input area. If you omit this step, the colorizer will not function properly.

Since we’re using a land cover GeoTIFF with specific pixel values mapping directly to land cover classifications we want to use `raster-colorizer-default-mode: exact` meaning stops will map to exact pixel values, and no other color values will be assigned through interpolation.

Now all that remains is to translate the land use data key into CartoCSS style rules, using the `raster-colorizer` stop syntax:

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
	

###### Finished Map

![](http://farm9.staticflickr.com/8094/8495387917_8425ce6b97_o.jpg)

#### 地形数据制图（Working with terrain data）

To get good-looking terrain maps, we usually combine several types of visualizations generated by the [GDAL](https://www.mapbox.com/tilemill/docs/guides/gdal) DEM utilities.

##### DEM数据源（Digital Elevation Model sources）

There are many different data formats for storing and working with digital elevation models. In our examples we’ll be working with geotiffs. Here are some examples of high quality free datasets that are available in geotiff format:

###### SRTM

Data collected from NASA’s [Shuttle Radar Topography Mission](http://www2.jpl.nasa.gov/srtm/) is a high quality source of elevation data covering much of the globe. It is available free from NASA directly, however we recommend working with [CGIAR’s cleaned up version](http://srtm.csi.cgiar.org/), which is also free.

###### ASTER

Aster is another global DEM datasource. It has better coverage of the earth’s surface than SRTM, and is slightly higher-resolution, but contains more errors than CGIAR’s clean SRTM set. Errors are usually in the form of spikes or pits, and can be significant.

###### USGS NED

The US Geological Survey publishes a [National Elevation Dataset](http://ned.usgs.gov/) for the United States. It high resolution and frequently updated from a variety of sources.

##### Types of Visualizations

###### Color-relief (or hypsometric tint)

![](https://www.mapbox.com/tilemill/assets/pages/relief-example.png)

Color relief assigns a color to a pixel based on the elevation of that pixel. The result is not intended to be hysically accurate, and even if natural-looking colors are chosen the results should not be interpreted as representative of actual landcover. Low-lying areas are often given assigned green and yellow shades, with higher elevations blending to shades of grey, white, and/or red.

###### Hillshade (or shaded relief)

Hillshade visualization analyzes the digital elevation model to simulate a 3-dimensional terrain. The effect of sunlight on topography is not necessarily accurate but gives a good approximation of the terrain.

###### Slope

Slope visualization assigns a color to a pixel based on the difference in elevation between it and the pixels around it. We use it to make steep hills stand out and enhance the look of our terrain.

##### Visualizations with GDAL-DEM

###### Reprojecting the Data

Chances are your DEM datasource will not come in the Google Mercator projection we need - for example, SRTM comes as [WGS84](http://spatialreference.org/ref/epsg/4326/) (EPSG:4326) and USGS NED comes as [NAD83](http://spatialreference.org/ref/epsg/4269/) (EPSG:4269).

For our example we’ll be working with some NED data of the District of Columbia area. The following command will reproject the file to the proper projection - see [Reprojecting a GeoTIFF for TileMill](https://www.mapbox.com/tilemill/docs/guides/reprojecting-geotiff/) for more detailed information.

	
	gdalwarp -s_srs EPSG:4269 -t_srs EPSG:3785 -r bilinear dc.tif dc-3785.tif
	

In the case of reprojecting elevation data, the `-r bilinear` option is important because other resampling methods tend to produce odd stripes or grids in the resulting image.

###### Creating hillshades

Run this command to generate the shaded relief image:

	
	gdaldem hillshade -co compress=lzw dc-3785.tif dc-hillshade-3785.tif
	

The `-co compress=lzw` option will compress the TIFF. If you’re not concerned about disk space you can leave that part out. If you are using GDAL 1.8.0 or later you may also want to add the option `-compute_edges` in order to avoid a black pixel border around the edge of the image.

Our output looks like this:

![](https://www.mapbox.com/tilemill/assets/pages/hillshade-dc.png)

**Note:** If there is not a 1:1 relation between your vertical and horizontal units, you will want to use the-s (scale) option to indicate the difference. Since Google Mercator X & Y units are meters, and NED elevation is also stored in meters this wasn’t necessary for our example. If your elevation data is stored in feet, you might do this instead (since there are approximately 3.28 feet in 1 meter):

	
	gdaldem hillshade -s 3.28 -co compress=lzw dc-3785.tif dc-hillshade-3785.tif
	

###### Generating color-relief

Before you can create a color-relief image, you nee to decide what colors you want to assign to different elevations. This color configuration is created and stored in a specially formatted plain text file. Each line in the file should have four numbers - an elevation, and an RGB triplet of numbers from 0 to 255. To figure out the proper RGB values of a color, you can used the color selection tool of any image editor, or an online too such as [ColorPicker.com](http://www.colorpicker.com/).

Here is our example, saved to a file called ramp.txt:

	
	0 46 154 88
	1800 251 255 128
	2800 224 108 31
	3500 200 55 55
	4000 215 244 244
	

The above numbers define a gradient that will blend 5 colors over 4000 units of elevation. (Our units are meters, but we don’t need to specify that in the file.) They will translate to a color-to-elevation relationship that looks like this:

![](https://www.mapbox.com/tilemill/assets/pages/ramp-illustration-fs8.png)

To apply this color ramp to the DEM, use the `gdaldem color-relief` command:

	
	gdaldem color-relief input-dem.tif ramp.txt output-color-relief.tif
	

The area you are working with may not have such a wide range of elevations. You can check the minimum & maximum values of your DEM to adjust your ramp style accordingly. The `gdalinfo` command can tell us the range of values in the geotiff:

	
	gdalinfo -stats your_file.tif
	

Among other things, this will tell you the minimum, maximum, and mean values of your elevation data to help you make decisions about how to choose your colors. With elevations tweaked for our area around DC, this is what we get:

![](https://www.mapbox.com/tilemill/assets/pages/color-dc.png)

###### Generating slope shading

With the gdaldem tools, generating a slope visualization is a two step process.

First, we generate a slope tif where each pixel value is a degree, from 0 to 90, representing the slope of a given piece of land.

	
	gdaldem slope dc-3785.tif dc-slope-3785.tif
	

_The same note about horizontal and vertical scale as hillshades applies for slope._

The tif output by `gdaldem slope` has no color values assigned to pixels, but that can be achieved by feeding it through gdaldem color-relief with an appropriate ramp file.

Create a file called ‘slope-ramp.txt’ containing these two lines:

	
	0 255 255 255
	90 0 0 0
	

This file can then be referenced by by the color-relief command to display white where there is a slope of 0° (ie, flat) and display black where there is a 90° cliff (with angles in-between being various shades of grey). The command to do this is:

	
	gdaldem color-relief -co compress=lzw dc-slope-3785.tif slope-ramp.txt dc-slopeshade-3785.tif
	

Which gives us:

![](https://www.mapbox.com/tilemill/assets/pages/dc-slope.png)

##### 合并最终结果（Combining the final result in TileMill）

To combine these geotiffs in TileMill, we’ll make use of the ‘multiply’ image blending mode using the `raster-comp-op` property, which stands for “compositing operation”. Assuming you have added your three geotiffs as layers with appropriate IDs, the following style will work. You can adjust the opacity of the hillshade and slope layers to tweak the style.

	
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

![](https://www.mapbox.com/tilemill/assets/pages/combined-dc.png)

##### 关于性能（Raster performance）

If your rasters are many MBs in size, then an important optimization is to build image pyramids, or overviews. This can be done with the `gdaladdo` tool or in QGIS. Building overviews does not impact the resolution of the original image, but it rather inserts new reduced resolution image references inside the original file that will be used in place of the full resolution data at low zoom levels. Any processing of the original image will likely drop any internal overviews previously built, so make sure to rebuild them if needed. You can use the gdalinfo tool to check if your tiff has overviews by ensuring the output has the ‘Overviews’ keyword reported for each band.


