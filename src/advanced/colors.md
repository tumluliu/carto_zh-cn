### 4.2 地图配色技巧

#### 色彩理论与制图

地图配色会显著影响你运用数据所诉说的“故事”，认识到这点十分重要。本节将简单介绍地图配色背后的理论。我们先来看三种基本配色方案。

##### 渐变色方案

Sequential schemes order data from high to low, accenting the highest as a dark shade and the lowest as a light shade (or vice versa). Sequential schemes are best if you are mapping quantitative data and do not want to focus on one particular range within your data.

渐变色方案先将数据排序，用深色表达排位高的数据，用浅色表达排位低的数据。或者反过来对排位高的数据用浅色，排位低的数据用深色。这种配色方案最适合于（不需要特别强调某个区间的）定量数据。

![](https://www.mapbox.com/tilemill/assets/pages/sequential.png)

##### 分裂补色方案（Diverging Schemes）

Divergent schemes are best at highlighting a particular middle range of quantitative data. Pick two saturated contrasting colors for the extremes of the data, and the middle ranges blend into a lighter mix of the two. This is particularly great for accenting the mean of your data and exposing locations that significantly ‘diverge’ from the norm.

分裂补色方案特别适用于强调数据的特定区间。数据排序后，选择两种对比强烈的饱和色分别表达最高位和最低位的数据，中间部分用这两种颜色的混色来表达。这种配色方案适合用来强调数据中的均值，以及揭示明显偏离常规的数据。

![](https://www.mapbox.com/tilemill/assets/pages/diverging.png)

##### 定性色方案（Qualitative Schemes）

If you are working with qualitative data, such as ethnicity or religion, you want to pick a series of ‘unrelated’ colors. The trick is to pick a really nice color theme so your map looks great. You can also accent particular aspects of your data by your choice of color. For example, one strong dark color among a group of lighter colors will ‘pop’ out of the map, highlighting that particular facet of your data against all others.

如果处理的数据是定性数据，如种族、宗教等，用一组互不相关的颜色会比较合适。这时选一组漂亮的颜色可以让地图看起来很赞。另外，还可以通过选取特定的颜色来凸显数据中你想要强调的内容。譬如在一组浅色中，强烈的深色在地图上会非常醒目，这可以使数据的某些特点更加引人注意。

![](https://www.mapbox.com/tilemill/assets/pages/qualitative.png)

#### 常用的取色工具

幸运的是有一些网站可用来搜寻色彩主题。以下是其中一些的链接及主要功能介绍。制图艺术博大浩瀚，它们可以帮你做出更明智的选择。本节末尾将评介两个桌面版的付费工具。

##### 0\_255

[0\_255](http://www.0to255.com/)是一个生成渐变色带的在线工具。它可以为同一色系生成一组亮度不同的颜色，因此非常适用于渐变色方案。它为任意一种颜色都提供了从全黑（`#000`）到全白（`#FFF`）共32种亮度渐变的不同颜色。对于生成的每种颜色，都可以直接拷贝其十六进制的颜色值。用户可以先从一个很大的颜色网格中选取一种初始色，然后0\_255就可以基于这种颜色生成一条亮度渐变的色带。如果在颜色网格中没有用户想要的颜色，那么也可以通过把自定义的十六进制颜色值粘贴进去来选取自己的颜色。

![](https://www.mapbox.com/tilemill/assets/pages/0_255_0.jpg)

##### Color Scheme Designer

Color Scheme Designer allows you to select a color from a color wheel and presents several options for automatically generating colors complementary to your initial choice. Specifically, you can choose between:

Color Scheme Designer允许用户在色盘上选取一种颜色，然后提供一些选项以自动生成所选颜色的对比色。具体而言，用户可以选择以下五种生成对比色的模式：（是否根据palette.com更新？）

- 补色（Complements）
- 三色组（Triads）
- 四色组（Tetrads）
- 相似（Analogic）
- 相似改（Accented Analogic）

在用户选定了初始色和模式之后，就可以得到一个包含了几种变化的颜色表，其中的每个颜色值都有十六进制的编码。

![](https://www.mapbox.com/tilemill/assets/pages/colorschemedesigner_1.jpg)

##### Colour Lovers

Colour Lovers is great if you are looking for a design theme for mapping qualitative data. Active users contribute palettes to the site, and these palettes are searchable, browsable, and ready to be used on your project. Colour Lovers users also post patterns if you need some spatial inspiration as well.

Colour Lovers是个为定性数据搜寻配色方案的出色应用。很多活跃的用户为其贡献自定义的色板。这些色板可以被搜索、浏览、甚至直接用于你的项目中。那么还可以从Colour Lovers上找到其他用户发布和分享的纹样。（spatial inspiration 不知所云，不如不翻）

![](https://www.mapbox.com/tilemill/assets/pages/colourlovers_0.jpg)

##### Kuler

Kuler是Adobe公司旗下一个与Colour Lovers功能相似的网站。设计师们在网站上发布自己的色系，颜色值由RGB或十六进制表示。Kuler网站的设计略不如Colour Lovers直观，但仍然值得一试。

![](https://www.mapbox.com/tilemill/assets/pages/kuler_1.jpg)

活跃的用户社区在Kuler上贡献了许多很棒的想法。Kuler还提供了有用的外部资源链接，包括论坛、在线帮助，若干关于颜色和色彩理论重要性的好文。

##### Colorbrewer

Colorbrewer is great for contrasting the three kinds of color models mentioned in the first section of this article. It allows you to test out different color themes based on whether you want sequential, diverging, or qualitative schemes and to vary the number of color classes you want in your pallet (up to 12). It also provides some useful ‘further reading’ articles on these theories and other cartographic design ideas.

Colorbrewer是个用来对比本节之前提到的三类色彩方案的好工具。它可以对不同颜色方案进行对比和测试，包括渐变色、分裂补色和定性色等方案，支持最多12种选定颜色。此外，它还提供了一些关于色彩理论和制图设计思想方面的文章供延伸阅读。

![](https://www.mapbox.com/tilemill/assets/pages/colorbrewer.jpg)

##### Pochade

Pochade是一个取色工具，能够获取屏幕上任意位置的颜色值（RGB值或十六进制值）。它支持通过HSB、RGB或CYMK等多种模型来修改颜色。Pochade是付费软件，价格9.99美元。

![](https://www.mapbox.com/tilemill/assets/pages/pochade_0.jpg)

##### ColorSchemer Studio

ColorSchemer Studio provides three main features: (1) a color class generator (up to 254 - many more than Color Brewer) based on two colors of your choice, (2) a ‘PhotoSchemer’, which allows you to upload a photo and determine up to ten different colors on your chosen photo and (3) integration with colourlovers, including a color browser and the ability to load colors into the first tool to manipulate on your own. ColorSchemer is a powerful tool for your desktop, available for the slightly higher price of $49.99.

ColorSchemer Studio主要有三个功能：(1) 基于两种预选颜色的色系生成器（支持最多254种颜色，远远多于Colorbrewer）；(2) “PhotoSchemer”，可以从现有的图片中提取色系；(3) 与Colour Lovers集成，包括一个颜色浏览器和向第一个工具（译注：first tool指的是什么？（yarray注：我也不知道是什么……，要不删了？））中加载颜色的能力。ColorSchemer是个强大的桌面应用，但价格稍贵，要49.99美元。

#### 参考文献

1. Mapbox, [Tips for using color in maps](https://www.mapbox.com/tilemill/docs/guides/tips-for-color/)
