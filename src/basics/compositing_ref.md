### 关于合成操作（Compositing）

_译注：[原文地址](https://www.mapbox.com/mapbox-studio/compositing-reference/)\_

Compositing operations affect the way colors and textures of different elements and styles interact with each other.

合成操作会改变不同要素与样式的颜色、纹理之间相互作用的方式。

Without any compositing operations on a source it will just be painted directly over the destination – compositing operations allow us to change this. There are 33 compositing operations available in CartoCSS:

如果不在_源_上应用合成操作，那么它就会被直接覆盖绘制在_目标_之上。而合成操作允许我们改变这种绘制行为。CartoCSS一共支持33种合成操作：

| CartoCSS支持的合成操作列表    |||
|-------------|---------------|----------|
| plus        | difference    | src      |
| minus       | exclusion     | dst      |
| multiply    | contrast      | src-over |
| screen      | invert        | dst-over |
| overlay     | invert-rgb    | src-in   |
| darken      | grain-merge   | dst-in   |
| lighten     | grain-extract | src-out  |
| color-dodge | hue           | dst-out  |
| color-burn  | saturation    | src-atop |
| hard-light  | color         | dst-atop |
| soft-light  | value         | xor      |

The operations in the first two columns are color blending modes that provide a variety of ways to control the blending of the colors of objects and layers with each other. The operations in the last column are [Duff-Porter alpha blending modes](http://www.imagemagick.org/Usage/compose/#duff-porter). They provide a variety of ways to fill and mask objects and layers with each other.

左边两列是色彩混合模式，它为对象与对象、图层与图层之间在颜色上如何融合提供了一系列不同方式。而最右边一列中的[Duff-Porter透明度混合模式](http://www.imagemagick.org/Usage/compose/#duff-porter)则提供的是一系列关于填充和遮盖的不同方式。

If you are familiar with image editors such as the GIMP or PhotoShop you will recognize many of these as layer blending modes. They work much the same way in Mapbox Studio, but do not (necessarily) operate on the layer as a whole. There are two ways to invoke a composite operation - on an entire style attachment via the `comp-op` property, or on a particular symbolizer via a symbolizer-specific property:

如果你对图像编辑软件（比如GIMP、PhotoShop）比较熟悉，那么应该能感觉到上面这些模式很多都和图层混合模式很像。它们的确很相似，但不同的是，CartoCSS中的合成操作却可以不必作用于整个图层。具体而言，CartoCSS的合成操作有两种使用方式：一是通过`comp-op`属性作用于一整个从属样式块；二是通过以下这些针对特定符号的合成操作属性作用于某种符号：

- line-comp-op
- line-pattern-comp-op
- marker-comp-op
- point-comp-op
- polygon-comp-op
- polygon-pattern-comp-op
- raster-comp-op
- shield-comp-op
- text-comp-op

There are times when you’ll want to use the style-wide comp-op and times when you’ll want to use the symbolizer-specific properties. It will depend on the results you want to achieve. With the symbolizer-specific approach, overlapping objects in the style will have their compositing operations applied to each other as well as the layers below. With the style-wide approach, the style will be rendered and flattened first.

这两种方式都很常用，具体使用哪种要看用户自己想要得到哪种渲染效果。它们的区别在于：如果采用方式一，那么地图会先按照所定义的样式进行渲染和平坦化（译注：不太确定flattened是个什么处理，达到了什么效果？）；而如果是方式二，那么不仅是样式中那些有重叠部分的对象之间，而且连当前图层下面的图层都会被应用合成操作。两种方式的差异见下面这个例子。

![](https://cloud.githubusercontent.com/assets/83384/3881031/1d351f02-218a-11e4-8092-10002ca9ff2b.png)

	
	// 方式一：style-wide
	#countries {
	  line-color: #345;
	  line-width: 4;
	  polygon-fill: #fff;
	  comp-op: overlay;
	}
	

![](https://cloud.githubusercontent.com/assets/83384/3881032/1d416118-218a-11e4-99e9-e29173c8b169.png)

	
	// 方式二：symbolizer-specific
	#countries {
	  line-color: #345;
	  line-width: 4;
	  line-comp-op: overlay;
	  polygon-fill: #fff;
	  polygon-comp-op: overlay;
	}
	

_Note_: When we talk about the effects of composite operations, we need to talk about a _source_ and a _destination_. The _source_ is the style or symbolizer that the `comp-op` property is applied to, and the _destination_ is the rest of the image that is drawn below that. There may also be more parts to the image that appear above the _source_; these are not affected by the `comp-op` and are drawn normally.

_注意：_我们在说合成操作的时候，需要明确_源_与_目标_。所谓_源_，就是应用了`comp-op`属性的样式或符号，而所谓_目标_，则是其它那些绘制在它下面的图像。在_源_的上面当然还可能有其它要绘制的部分，但它们都不会受到当前_源_中`comp-op`的影响，该怎么画就还怎么画。

#### 色彩混合（Color Blending）

There are 22 color-blending compositing operations. This section will describe the ones that are most useful for cartographic design in Mapbox Studio. To illustrate the differences between them all, we’ll show how each of them affect a few example layers and backgrounds.

色彩混合合成操作一共有22种。这里对其中那些在制图设计中最常用的进行简单介绍。为了更明显的表现出不同模式之间的差异，我们将这些操作应用在两个示例图层和两幅背景图上。

These are the layers the `comp-op` properties will be applied to:

下面是两个示例图层：

![](https://cloud.githubusercontent.com/assets/83384/3881387/233691b2-218d-11e4-992f-3e58df40febd.png)![](https://cloud.githubusercontent.com/assets/83384/3881393/234c2e32-218d-11e4-846c-e92949653007.png)

These are the backgrounds the `comp-op` layers will be overlaid on:

下面是两幅被`comp-op`图层叠加的背景图：

![](https://cloud.githubusercontent.com/assets/83384/3881341/22ac38c8-218d-11e4-8e3a-52f3b5e4e048.png)![](https://cloud.githubusercontent.com/assets/83384/3881342/22ac91d8-218d-11e4-9f95-c5ecf675a608.png)

Here is what the result looks like with no `comp-op` property applied:

下面是在不定义`comp-op`属性时的渲染效果：

![](https://cloud.githubusercontent.com/assets/83384/3881398/236277aa-218d-11e4-813d-418292c58223.png)![](https://cloud.githubusercontent.com/assets/83384/3881381/2326cb06-218d-11e4-9295-92c99e33eac3.png)

##### Overlay

![](https://cloud.githubusercontent.com/assets/83384/3881377/2318eef0-218d-11e4-94c0-0c58a5995a33.png)![](https://cloud.githubusercontent.com/assets/83384/3881381/2326cb06-218d-11e4-9295-92c99e33eac3.png)

The `overlay` comp-op combines the colors from the source image, and also uses them to exaggerate the brightness or darkness of the destination. Overlay is one of a few composite operations that works well for texturing, including using it for terrain data layers.

`overlay`模式采用源图像中的颜色，并用这些颜色去夸大目标图像中的明暗度（译注：十分不确定这句话的意思）。`overlay`模式是几种可良好用于表现纹理的合成操作之一，可用于地形数据图层的样式配置。

##### Multiply

![](https://cloud.githubusercontent.com/assets/83384/3881379/231d32ee-218d-11e4-9627-cc17d776f0a3.png)![](https://cloud.githubusercontent.com/assets/83384/3881376/23164fba-218d-11e4-9913-7d37b4ce98bd.png)

The `multiply` comp-op multiplies the color of the source and destination, usually resulting in a darkened image tinted to the color of the source. If either the source or destination is solid white, the other will appear unchanged. If either the source or destination is solid black, the result will also be solid black.

`multiply`合成操作将源与目标的颜色进行相乘运算。这种操作得到图像的颜色通常是基于源图像颜色的暗化效果。如果源（或目标）是纯白色的，那么合成的效果就是保持目标（或源）的颜色不变。如果源或目标是纯黑色的，那么合成的效果就是纯黑色。

One of the many uses for multiply is to simulate the way ink colors would blend with each other or with a textured surface. It can also be used for other kinds of texure effects.

`multiply`操作的众多应用之一是模拟不同颜色的墨水混合或墨水与带有纹理的表面混合的效果。此外，它还可以用于其它纹理效果。

##### Color-dodge

![](https://cloud.githubusercontent.com/assets/83384/3881330/228f3ea8-218d-11e4-9a26-d9c6c501f4ec.png)![](https://cloud.githubusercontent.com/assets/83384/3881334/2296b2b4-218d-11e4-9ca7-a5ac240da41e.png)

The `color-dodge` comp op brightens the colors of the destination based on the source. The lighter the source, the more intense the effect. You’ll get nicer results when using this on dark to mid-tone colors, otherwise the colors can become too intense.

`color-dodge`操作以源图像为基础将目标的颜色亮化。源图像的颜色越亮，合成后的效果越刺眼。要想得到比较美观的效果，需要在颜色较暗的颜色上应用该操作，否则就会得到过于刺眼的合成效果。

##### Plus

![](https://cloud.githubusercontent.com/assets/83384/3881380/2321f054-218d-11e4-9ca6-94594a077f3a.png)![](https://cloud.githubusercontent.com/assets/83384/3881383/23286c2c-218d-11e4-97ef-c252e411b007.png)

The `plus` comp-op adds the color of the source to the destination. For example, if your source color is dark red, this operation will add a small amount of red color to the destination causing it to brighten and also turn red. The lighter your source color, the lighter your result will be because a lot of color will be added. A completely black source will not affect the destination at all because no color will be added. Using this mode on darker source layers is recommended.

`plus`操作将源的颜色与目标颜色相加。举个例子，如果源的颜色是深红（dark red），那么`plus`操作就会为目标增加少量的红色，使其亮度增加而且颜色偏红。源的颜色越浅，得到结果的颜色也会越浅，因为会有很多颜色被添加（译注：神马意思？？）。纯黑色的源不会对目标产生任何影响，因为没有颜色会被叠加（译注：这里的“颜色”指的是RGB中的颜色分量吗？黑色是#000，所以“no color”；白色是#FFF，所以有“a lot of color”，是这个意思？）。推荐在源图层颜色较深的时候使用这个合成操作。

##### Minus

![](https://cloud.githubusercontent.com/assets/83384/3881374/230be6c4-218d-11e4-86c3-220891079924.png)![](https://cloud.githubusercontent.com/assets/83384/3881375/230e6962-218d-11e4-9cf5-485a1a7a355d.png)

The `minus` comp-op subtracts the color of the source from the destination. For example, if your source color is a dark red, this operation will remove a small amount of red color from the destination causing it to darken and turn slightly green/blue. The lighter your source color, the darker your result will be because a lot of color will be subtracted. A completely black source will not affect the destination at all because no color will be removed. Using this mode on darker source layers is recommended.

`minus`操作从目标层的颜色中减去源层的颜色。举个例子，如果源层颜色是深红（dark red），那么这个操作会从目标层中减少一部分红色，使其亮度变暗而且颜色偏绿/蓝。源层颜色越浅，得到的结果颜色越深，因为会有更多的颜色被减掉。纯黑色的源不会对目标产生任何影响，因为没有任何颜色分量被减掉。推荐在源层颜色较深的时候使用这个合成操作。

In the bathymetry example above there are more polygons overlapping each other. The subtraction is run for each overlapping piece, causing areas with a lot of overlap to darken more and shift more to the green spectrum.

在上面海水背景叠加的例子中，有更多相互叠加的多边形。减操作会被作用于每个叠加的部分，从而引起重叠很多的区域亮度更暗，颜色更偏绿色。

##### Screen

![](https://cloud.githubusercontent.com/assets/83384/3881386/23350540-218d-11e4-9053-a73c7e524e24.png)![](https://cloud.githubusercontent.com/assets/83384/3881384/2331a3f0-218d-11e4-9982-d96365a72c9d.png)

The `screen` comp-op will paint white pixels from the source over the destination, but black pixels will have no affect. This operation can be useful when applied to textures or raster layers.

`screen`操作是把源层中的白色像素点画到目标层上，而黑色像素点则不会有效果。这个操作适用于纹理或栅格图层。

##### Darken

![](https://cloud.githubusercontent.com/assets/83384/3881336/229dba14-218d-11e4-9750-369326b72e7f.png)![](https://cloud.githubusercontent.com/assets/83384/3881339/22a5153e-218d-11e4-9c84-4adc8d80f5fc.png)

The `darken` comp-op compares the individual red, green, and blue components of the source and destination and takes the lower of each. This operation can be useful when applied to textures or raster layers.

`darken`操作会对源层与目标层颜色中的R、G、B分量分别比较，然后取较小的值作为合成结果的颜色分量。这个操作适用于纹理或栅格图层。

##### Lighten

![](https://cloud.githubusercontent.com/assets/83384/3881372/2306ffec-218d-11e4-8a9a-16ce1357e9f5.png)![](https://cloud.githubusercontent.com/assets/83384/3881373/230a2758-218d-11e4-8ed7-219d8b04882a.png)

The `lighten` comp-op compares the individual red, green, and blue components of the source and destination and takes the higher of each.

`lighten`操作会对源层与目标层颜色中的R、G、B分量分别比较，然后取较大的值作为合成结果的颜色分量。

##### Color-burn

![](https://cloud.githubusercontent.com/assets/83384/3881332/2290e28a-218d-11e4-8b95-fc62ab495ed9.png)![](https://cloud.githubusercontent.com/assets/83384/3881333/2295bd78-218d-11e4-957e-c9076d47dccf.png)

The `color-burn` comp op darkens the colors of the destination based on the source. The darker the source, the more intense the effect.

`color-burn`操作基于源层颜色对目标层颜色进行加深。源层的颜色越深，合成结果就会越强烈。（译注：intense这里该译成什么？）

##### Hard-light

![](https://cloud.githubusercontent.com/assets/83384/3881364/22ec6f92-218d-11e4-8c25-fa4691a457e6.png)![](https://cloud.githubusercontent.com/assets/83384/3881365/22f03b54-218d-11e4-8d2b-afcb577fee0f.png)

The `hard-light` comp-op will use light parts of the source to lighten the destination, and dark parts of the source to darken the destination. Mid-tones will have less effect

`hard-light`操作会用源层中较明亮的部分去调亮目标层的亮度，用源层中较暗的部分去调暗目标层的亮度，而那些中间色调的颜色则不会有太多影响。

##### Soft-light

![](https://cloud.githubusercontent.com/assets/83384/3881389/233a064e-218d-11e4-8b11-1f21caa0e5ed.png)![](https://cloud.githubusercontent.com/assets/83384/3881388/23372320-218d-11e4-84b6-75eec4af462f.png)

The `soft-light` comp-op works like a less intense version of the overlay mode. It is useful for applying texture effects or ghost images.

`soft-light`操作可以看作是`overlay`操作的一个弱化版本。它可以用于产生纹理效果或虚影图像。

##### Grain-merge

![](https://cloud.githubusercontent.com/assets/83384/3881361/22e3dcf6-218d-11e4-9e56-90f7aeb94367.png)![](https://cloud.githubusercontent.com/assets/83384/3881362/22e97792-218d-11e4-9082-f8a8ca814d5b.png)

##### Grain-extract

![](https://cloud.githubusercontent.com/assets/83384/3881358/22db97a8-218d-11e4-9157-d913f9b523f8.png)![](https://cloud.githubusercontent.com/assets/83384/3881363/22eb42ac-218d-11e4-97c5-bb8d9fb16e8d.png)

##### Hue

![](https://cloud.githubusercontent.com/assets/83384/3881366/22f57876-218d-11e4-96e1-88fb26a181c6.png)![](https://cloud.githubusercontent.com/assets/83384/3881367/22f64c9c-218d-11e4-9798-e1cdf719502b.png)

The `hue` comp-op applies the hue of the source pixels to the destination pixels, keeping the destination saturation and value.

`hue`操作将源层中每个像素的色调应用于目标层的对应像素中，而保持饱和度与明度不变（译注：即在HSV颜色模型中，应用H，而保持S和V不变）。

##### Saturation

![](https://cloud.githubusercontent.com/assets/83384/3881382/2327d55a-218d-11e4-9056-620382c07de3.png)![](https://cloud.githubusercontent.com/assets/83384/3881385/233295da-218d-11e4-87c4-9922c2bd9c22.png)

The `saturation` comp-op applies the saturation of the source pixels to the destination pixels, keeping the destination hue and value.

`saturation`操作将源层中每个像素的饱和度应用于目标层的对应像素中，而保持色调与明度不变（译注：即在HSV颜色模型中，应用S，而保持H和V不变）。

##### Color

![](https://cloud.githubusercontent.com/assets/83384/3881331/228fddf4-218d-11e4-87a5-d98f1ef742f7.png)![](https://cloud.githubusercontent.com/assets/83384/3881329/228b0f22-218d-11e4-80da-4d1ef13490a0.png)

The `color` comp-op applies the saturation of the source pixels to the destination pixels, keeping the destination hue and value.

`color`操作将源层中每个像素的饱和度应用于目标层的对应像素中，而保持色调与明度不变（译注：即在HSV颜色模型中，应用S，而保持H和V不变。可是可是，这个和上边的`saturation`操作的解释怎么完全一样？）。

##### Value

![](https://cloud.githubusercontent.com/assets/83384/3881399/2363996e-218d-11e4-866c-acfdda0d7e0e.png)![](https://cloud.githubusercontent.com/assets/83384/3881402/2368f206-218d-11e4-9f13-3ab2e694f92f.png)

The `value` comp-op applies the value of the source pixels to the destination pixels, keeping the destination hue and saturation.

`value`操作将源层中每个像素的明度应用于目标层的对应像素中，而保持色调与饱和度不变（译注：即在HSV颜色模型中，应用V，而保持H和S不变）。

#### 透明度混合（Alpha Blending）

There are 11 alpha blending compositing operations. Rather than altering the colors of a layer, these operations use the shapes of a layer to show or hide the rest of the image in different ways.

透明度混合合成操作一共有11种。不同于颜色混合操作是对层的颜色进行修改，透明度混合主要利用层与层之间的形状关系来显示或隐藏渲染图像的某些部分，且能够支持很多种不同的显示或隐藏方式。

Some of these modes will be more useful when applied to the whole style with the `comp-op` property, rather than with a symbolizer-specific property such as `polygon-comp-op`. All of the examples below were created with `comp-op`; there would be fewer differences between some of them had `polygon-comp-op` been used.

在这些透明度混合操作中，有一些更适用于通过`comp-op`属性应用于整个样式块，而非用于特定符号的合成属性（如`polygon-comp-op`）。本小节的例子全部都是通过`comp-op`属性实现的。它们其中有一些如果应用`polygon-comp-op`属性的话，效果会有些许不同。

The `src` and `dst` composite operations show only the source and destination layers, respectively. Neither are of much use in Mapbox Studio (where you can just as easily hide the layers). The `src-over` comp-op is another one you won’t be uding (**typo**) much. It draws the source and destination normally, the same as not applying a comp-op at all. The rest of the alpha blending compositing operations may be useful for cartography, however.

`src`和`dst`操作的含义是分别只显示源层和目标层。在实际制图中，这两种操作很少被用到（因为可以通过打开或关闭是否隐藏图层的开关来实现一样的效果）。`src-over`也是一个基本不会被用到的操作，因为它的含义就是按顺序正常绘制源层和目标层，和不使用`comp-op`属性效果完全一样。剩下的其它8种透明度混合操作在制图中就比较有用了，下面来一一介绍。

##### Dst-over

![](https://cloud.githubusercontent.com/assets/83384/3881350/22c57d1a-218d-11e4-9e97-ccb28a19d971.png)![](https://cloud.githubusercontent.com/assets/83384/3881351/22c7d876-218d-11e4-9002-11ba7ca66adc.png)

The `dst-over` comp-op will draw the source beneath everything else. If your destination forms a solid background, this will effectively hide the source.

`dst-over`操作会将源层绘制在最底层。如果目标层是不透明的背景，那么这个操作的效果就是把源层隐藏起来。

##### Src-in

![](https://cloud.githubusercontent.com/assets/83384/3881394/234dd5a2-218d-11e4-8bfa-279ce07573f8.png)![](https://cloud.githubusercontent.com/assets/83384/3881392/2349ad74-218d-11e4-84e7-3e6f9261bbdf.png)

The `src-in` comp-op will only draw parts of the source if they intersect with parts of the destination. The colors of the destination will not be drawn, only alpha channel (the shapes). If your destination forms a solid background, this operation will effectively be the same as `src`, since all parts of the source will intersect with the destination.

`src-in`操作的效果是只绘制出源层中与目标层相交叠的部分。目标层中除了Alpha通道以外的颜色值都不会被绘制。如果目标层只是一个单纯的不透明背景，那么这个操作的效果就和`src`一样，因为源层中的所有部分都会和目标层相交叠。

##### Dst-in

![](https://cloud.githubusercontent.com/assets/83384/3881344/22b87980-218d-11e4-891d-cfd326d518ef.png)![](https://cloud.githubusercontent.com/assets/83384/3881346/22bcd0de-218d-11e4-8a10-30df1c5fa612.png)

The `dst-in` comp-op will only draw parts of the destination that intersect with parts of the sources. The colors of the source will not be drawn, only the alpha channel (the shapes). If your source is completely solid, this operation will effectively be the same as `dst`, since all parts of the destination will intersect with the source.

`dst-in`操作是只绘制目标层中与源层交叠的被ufen。源层中除了Alpha通道以外的颜色值都不会被绘制。如果源层完全不透明，那么这个操作就和`dst`效果一样，因为目标层的所有部分都会和源层相交叠。

##### Src-out

![](https://cloud.githubusercontent.com/assets/83384/3881396/2354f404-218d-11e4-8568-49f82ca4162f.png)![](https://cloud.githubusercontent.com/assets/83384/3881395/2354c0d8-218d-11e4-936e-bce3adf1f3fd.png)

The `src-out` comp-op will only draw parts of the source that do not intersect parts of the destination. The colors of the destination will not be drawn, only alpha channel (the shapes). If your destination forms a solid background, this operation will completely hide both the source and the destination, since all parts of the source intersect the destination.

`src-out`操作的效果是只绘制源层中那些与目标层不相交的部分。目标层中除了Alpha通道以外的颜色值都不会被绘制。如果目标层只是一个单纯的不透明背景，那么这个操作的效果就是将源层与目标层都隐藏掉，因为源层的所有部分都与目标层相交。

##### Dst-out

![](https://cloud.githubusercontent.com/assets/83384/3881347/22bde7a8-218d-11e4-832d-538c601a88f3.png)![](https://cloud.githubusercontent.com/assets/83384/3881349/22be78da-218d-11e4-980f-ed4073e842ff.png)

The `dst-out` comp-op will only draw parts of the destination that do not intersect parts of the source. The colors of the source will not be drawn, only alpha channel (the shapes). If your source is completely solid, this operation will completely hide both the source and the destination, since all parts of the source intersect the destination.

`dst-out`操作的效果是只绘制目标层中那些与源层不相交的部分。源层中除了Alpha通道以外的颜色值都不会被绘制。如果源层完全不透明，那么这个操作的效果就是将源层与目标层都隐藏掉，因为源层的所有部分都与目标层相交。

##### Src-atop

![](https://cloud.githubusercontent.com/assets/83384/3881390/23448290-218d-11e4-87cb-9959b68357fc.png)![](https://cloud.githubusercontent.com/assets/83384/3881391/2345454a-218d-11e4-9f3b-b94b6d97c3b3.png)

（**译注：第一张图看起来有点问题，怎么地理范围都变化了？**）

The `src-atop` comp-op will only draw the source where it intersects with the destination. It will also draw the entire destination. If your destination forms a solid background, the result will be the same as `src-over` (or no comp-op at all).

`src-atop`操作的效果是只绘制源层中与目标层相交叠的部分。但它也会把整个目标层都画出来。如果目标层只是个不透明的背景，那么效果就和`src-over`（或者不应用合成操作）一样。

##### Dst-atop

![](https://cloud.githubusercontent.com/assets/83384/3881343/22b63abc-218d-11e4-989d-998bd386dbdf.png)![](https://cloud.githubusercontent.com/assets/83384/3881345/22bafc78-218d-11e4-9fc2-8bf14c5cd158.png)

The `dst-atop` comp-op will only draw the destination on top of the source, but only where the two intersect. All parts of the source will be drawn, but below the destination. If your destination forms a solid background, no part of the source will be visible.

`dst-atop`操作的效果是只将目标层中与源层相交的部分绘制在源层的上面，而源层的所有部分都会被绘制在目标层的下面。如果目标层是一个不透明背景，那么目标层就会完全不可见。

##### Xor

![](https://cloud.githubusercontent.com/assets/83384/3881400/2366cd96-218d-11e4-9047-afacbbec5a53.png)![](https://cloud.githubusercontent.com/assets/83384/3881401/2367757a-218d-11e4-8460-9fc2e5c50e42.png)

The `xor` comp-op means ‘exclusive or’. It will only draw parts of the source and destination that do not overlap each other. If either your source or your destination forms a solid layer, neither will be drawn because there are no non-overlapping parts.

`xor`，也就是“异或”操作的效果是只绘制源层与目标层不相交的部分（译注：相交的部分似乎是作全透明处理了，待考证）。如果源层或目标层中有一个是完全不透明的，那么结果将是两个层都不会被画出来，因为二者没有不相交的部分。
