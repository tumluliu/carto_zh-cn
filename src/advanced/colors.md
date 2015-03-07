### 色彩使用技巧

_译注：[原文地址](https://www.mapbox.com/tilemill/docs/guides/tips-for-color/)_

#### 色彩理论与制图（Color Theory and Mapping）

It’s important to realize how much your choice of color on a map can affect the story you are trying to tell with your data. As a brief introduction to some theories behind how to use colors on maps, here are three primary scheming designs for mapping data.

为地图选择的配色方案会在很大程度上影响你利用地图来讲述的关于数据的故事的精彩程度。本节内容是关于地图配色理论的简介，包括了三种用于展示地图数据的基本色彩设计方案。

##### 连续色方案（译注：还是“渐变色方案”？）（Sequential Schemes）

Sequential schemes order data from high to low, accenting the highest as a dark shade and the lowest as a light shade (or vice versa). Sequential schemes are best if you are mapping quantitative data and do not want to focus on one particular range within your data.

连续色方案先将地图数据依次排序，然后对高位数据用深色、低位数据用浅色表示（或者反过来）。这种配色方案特别适用于地图数据是数值类型而且不需要对数据中的某个区间特别强调的情况。

![](https://www.mapbox.com/tilemill/assets/pages/sequential.png)

##### 发散色方案（Diverging Schemes）

Divergent schemes are best at highlighting a particular middle range of quantitative data. Pick two saturated contrasting colors for the extremes of the data, and the middle ranges blend into a lighter mix of the two. This is particularly great for accenting the mean of your data and exposing locations that significantly ‘diverge’ from the norm.

发散色方案特别适用于突出强调数据中的某个中间区间。要构造这种方案，需要先选择两种饱和度对比色作为色带的两端，然后基于这两个端点颜色的混色构造色带的中间部分。这种配色方案适合用来强调显示数据中的均值，以及那些不符合正常分布的毛刺数据（译注：这两个半句的意思不矛盾吗？）

![](https://www.mapbox.com/tilemill/assets/pages/diverging.png)

##### 定性配色方案（Qualitative Schemes）

If you are working with qualitative data, such as ethnicity or religion, you want to pick a series of ‘unrelated’ colors. The trick is to pick a really nice color theme so your map looks great. You can also accent particular aspects of your data by your choice of color. For example, one strong dark color among a group of lighter colors will ‘pop’ out of the map, highlighting that particular facet of your data against all others.

如果处理的数据是定性数据，比如种族、宗教等这种属性数据，那么用一组互不相关的颜色会比较合适。遇到这种情况时，选一组漂亮的颜色可以让地图看起来很赞。另外，还可以通过选取特定的颜色来凸显数据中你想要强调的内容。举个例子，在一组较浅的颜色中，使用深黑色标记的部分会在地图上很乍眼，从而可以这部分特定的数据更加引人注意。

![](https://www.mapbox.com/tilemill/assets/pages/qualitative.png)

#### 常用的取色工具（Sources for Color-Picking）

Fortunately a number of sites are available to explore color themes. Below are links to some of these with a brief synopsis of their main features that will help you more easily navigate the seas of storytelling via aesthetics. The section ends with two reviews of purchasable tools that you can use from your desktop.

幸运的是，有不少网站和工具可以帮助我们生成配色方案。这里简单介绍几个这样的网站以及其主要功能。它们可以帮助你更方便的在颜色的艺术海洋中找到最适合讲述自己故事的色彩。除了一些网站，最后还会介绍两个桌面应用，不过它们是付费软件。

##### 0\_255

0\_255 is a great site for picking between different shades of one color, so it’s ideal for sequential schemes. It gives you thirty options for any given color, allowing you to instantly copy the color’s hex code. There is a large grid of colors to select from for an initial color, and then 0\_255 visualizes a range of shades based on that color. If the grid does not have the color you want, you can pick your own color by pasting in the hex code of your chosen color.

0\_255可以在同一色系中选取一组亮度不同的颜色，因此非常适合生成连续色方案。它为任意颜色都提供了30种选项，还可以直接拷贝颜色的十六进制值。用户可以先从一个很大的颜色网格中选取一种初始色，然后0\_255就可以基于这种颜色生成一条亮度渐变的色带。如果在颜色网格中没有用户想要的颜色，那么也可以通过把自定义的十六进制颜色值粘贴进去来选取自己的颜色。

![](https://www.mapbox.com/tilemill/assets/pages/0_255_0.jpg)

##### Color Scheme Designer

Color Scheme Designer allows you to select a color from a color wheel and presents several options for automatically generating colors complementary to your initial choice. Specifically, you can choose between:

Color Scheme Designer通过让用户在一个色盘上选取颜色和配置来自动生成所选颜色的补色（译注：有专业术语吗？）。具体而言，用户可以选择以下五种生成补色的模式：

- Complements
- Triads
- Tetrads
- Analogic
- Accented Analogic

Once you’ve chosen your color and scheme, you’ll have a color table that gives you several variations of your colors and their corresponding hex codes.

在用户选定了初始色和模式之后，就可以得到一个包含了几种变化的颜色表，其中的每个颜色值都有十六进制的编码。

![](https://www.mapbox.com/tilemill/assets/pages/colorschemedesigner_1.jpg)

##### Colour Lovers

Colour Lovers is great if you are looking for a design theme for mapping qualitative data. Active users contribute palettes to the site, and these palettes are searchable, browsable, and ready to be used on your project. Colour Lovers users also post patterns if you need some spatial inspiration as well.

Colour Lovers是个为定性数据生成配色方案的出色应用。有很多活跃的用户为其贡献色板。在网站上可以对这些色板进行搜索和浏览，它们可以直接被用于你的项目中。如果你需要一些空间上的灵感，那么还可以从Colour Lovers上找到其他用户发布和分享的样式（译注：指那些可以用于填充多边形或背景的样式）。

![](https://www.mapbox.com/tilemill/assets/pages/colourlovers_0.jpg)

##### Kuler

Kuler is a service similar to Colour Lovers offered by Adobe. A community of designers submit their own themes, which are available for RGB values and hex codes. The design of the site is a little less intuitive than Colour Lovers, but still worth checking out.

Kuler是一个Adobe公司旗下的与Colour Lovers功能相似的网站。上面聚集的一些设计师会发布一些自己的方案，其中的颜色值由RGB或十六进制表示。Kuler网站的设计不如Colour Lovers直观，但仍然值得收藏。

![](https://www.mapbox.com/tilemill/assets/pages/kuler_1.jpg)

There are a lot of great ideas being posted by an active community of contributing users. Kuler also provides extensive links to things like forums, help pages, as well as several articles on the importance of color and color theory.

Kuler上面有非常多的很棒的想法。它们都是由一大群活跃的用户发布的。除此以外，Kuler还提供了很多有用的外部资源链接，包括论坛、在线帮助，以及多篇关于颜色和色彩理论重要性的好文。

##### Colorbrewer

Colorbrewer is great for contrasting the three kinds of color models mentioned in the first section of this article. It allows you to test out different color themes based on whether you want sequential, diverging, or qualitative schemes and to vary the number of color classes you want in your pallet (up to 12). It also provides some useful ‘further reading’ articles on these theories and other cartographic design ideas.

Colorbrewer是个用来对比本节之前提到的三类色彩方案的好工具。它可以对不同颜色方案进行对比和测试，包括连续色、发散色和定性配色等方案，支持最多12种选定颜色。此外，它还提供了一些关于色彩理论和制图设计思想方面的扩展阅读文章，值得一读。

![](https://www.mapbox.com/tilemill/assets/pages/colorbrewer.jpg)

##### Pochade

Pochade is a color picking program that allows you to determine the RGB value and hex code of any color you see on your screen. It also provides a few different ways of manipulating your color, such as changing its HSB, RGB, or CMYK values. This program is available for download for $9.99.

Pochade是一个取色工具，能够获取屏幕上任意位置的颜色值（RGB值或十六进制值）。它支持通过HSB、RGB或CYMK等多种模型来修改颜色。Pochade是付费软件，价格9.99美元。

![](https://www.mapbox.com/tilemill/assets/pages/pochade_0.jpg)

##### ColorSchemer Studio

ColorSchemer Studio provides three main features: (1) a color class generator (up to 254 - many more than Color Brewer) based on two colors of your choice, (2) a ‘PhotoSchemer’, which allows you to upload a photo and determine up to ten different colors on your chosen photo and (3) integration with colourlovers, including a color browser and the ability to load colors into the first tool to manipulate on your own. ColorSchemer is a powerful tool for your desktop, available for the slightly higher price of $49.99.

ColorSchemer Studio主要有三个功能：(1) 基于两种预选颜色的色系生成器（支持最多254种颜色，远远多于Colorbrewer）；(2) “PhotoSchemer”可以从一张现有的图片中提取色系；(3) 与Colour Lovers集成，包括一个颜色浏览器和向第一个工具（译注：first tool指的是什么？）中加载颜色的能力。ColorSchemer是个强大的桌面应用，但价格稍贵，要49.99美元。
