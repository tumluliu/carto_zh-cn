## 概述

CartoCSS是一种语法类似CSS（Cascading Style Sheets，层叠样式表，一种对网页进行设计的样式语言）的制图样式描述语言。如果熟悉CSS的话，那么CartoCSS这种对地图进行样式设计的语言也会看起来不陌生，尽管二者所包含的要素、属性等内容和含义完全不同。

译注：把关于符号和多符号的内容挪到基础用法部分中去了，准备在这里讲一下下面这三个内容，恐怕对吸引读者更有作用。

1. 为什么要用一种脚本语言来进行地图样式设计？这是否符合目前主流的设计（in general sense）工具潮流？其背后是不是隐藏着什么规律，无论是工业设计领域的，还是计算机软件设计领域的，还是制图设计和地理信息科学领域的一种一般性的规律？
2. 如果第一点的结论是“使用脚本语言进行地图制图设计是符合潮流与规律的先进方法”，那么该领域内是否还有其它制图脚本语言？它们的现在的状况如何？
3. 如果第二点中列出的其它制图脚本语言的状况不佳，那么为什么CartoCSS又有什么亮点呢？是哪些特点使得它值得我们关注？

如果上面三点阐述清楚了，那么接下来可以再介绍一下CartoCSS的诞生和发展过程。这里面就不得不提到Mapnik，这个尺度就不太好把握了。主要还是侧重于发展历程吧，即从Mapnik到Cascadenik，再到CartoCSS的发展过程。

最后，再介绍一下用CartoCSS制图与目前其它主流（比如ArcGIS，或者真正的地图出版社）制图方法及工具链的对比。这个也蛮有挑战的。

**2015.01.15. 注意：根据以下参考材料补充完善这部分内容**

- [http://www.macwright.org/2012/11/02/css-for-maps.html](http://www.macwright.org/2012/11/02/css-for-maps.html)
- [http://www.developmentseed.org/blog/2011/feb/09/introducing-carto-css-map-styling-language/](http://www.developmentseed.org/blog/2011/feb/09/introducing-carto-css-map-styling-language/)
- [https://github.com/mapbox/carto/blob/master/README.md](https://github.com/mapbox/carto/blob/master/README.md)

