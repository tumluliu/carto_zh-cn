# CartoCSS中文指南

## 前言

这是一份根据[Mapbox Studio的在线帮助文档](https://www.mapbox.com/mapbox-studio/style-quickstart/)、[TileMill的帮助文档](https://www.mapbox.com/tilemill/docs/crashcourse/introduction/)以及CartoCSS语言的开源解释器项目[carto](https://github.com/mapbox/carto)的文档（这也是Mapbox官方的CartoCSS Reference链接指向的文档）翻译、整理并适当补充和修订之后的中文版CartoCSS指南与语言参考手册。翻译整理的目的是为了方便越来越多的使用CartoCSS进行制图的中文用户了解这种制图样式语言中各种属性的含义及用法。读这份文档需要具有一些的地理信息系统（GIS）、地图制图学（Cartography）方面的背景知识。由于CartoCSS是为[Mapnik](https://github.com/mapnik/mapnik)而设计的，而且其脚本最终会被解译为Mapnik样式并进行制图渲染，所以如果知道并了解Mapnik的工作原理，那么会对CartoCSS的语法要素及工作机理有更深刻的理解，但这不是使用CartoCSS进行制图所必需的。关于背景知识，可以参考Mapnik项目中的[相关文档](https://github.com/mapnik/mapnik/wiki/LearningMapnik)。

看了前面一段，你可能已经被其中出现的除CartoCSS以外的其它几个名词搞晕了。我在这里简单解释一下它们都是什么，以及它们之间是什么关系。

- **Mapnik**是一个开源的地图渲染引擎，用C++语言开发，有Python和node.js接口。它的开发可以追溯到2005年，但直到2008年的0.5版本发布之后才真正展现出它的强大——渲染质量高，而且速度很快。Mapnik有一套基于XML的地图样式描述方法，但在描述较复杂地图样式的时候其XML样式也会变得很长很复杂，难以让人类直接阅读和修改，而这恰恰就是后来CartoCSS这种高级地图样式描述语言出现的一个重要原因。
- **Mapbox**是一个专注于数字化制图服务的公司，成立于2010年。它是目前能紧紧把互联网、云计算、移动计算和传统的地图制图设计结合起来而且结合的最好的公司之一。目前，它已经网罗了该领域中全世界能数的过来的众多牛人，这其中包括Mapnik的作者[Dane Springmeyer](https://github.com/springmeyer)，MBTiles的设计者[Tom MacWright](https://github.com/tmcw)和[Justin Miller](https://github.com/incanus)，其中Tom MacWright也是CartoCSS的设计者，还有OSRM的作者[Dennis Luxen](https://github.com/DennisOSRM)等等，不一而足。目前，Mapbox的发展突飞猛进，产品线逐渐拉长，业务范围向企业级私有空间数据基础设施服务等方向推进。这里要强调Mapbox的一个很重要的特点，就是它维护着大量的开源项目。注意Mapbox的官方网站在中国大陆地区是被GFW禁止访问的，所以要了解更多关于它的信息请自备梯子。
- **TileMill**和**Mapbox Studio**都是Mapbox维护的开源项目，都是跨平台的制图客户端软件，都是用node.js开发的，只是后者是最近发布的，有取代前者的意思，但目前这两个软件都可以使用。在TileMill和Mapbox Studio中进行制图需要使用CartoCSS语言，制好的地图可以导出成MBTiles格式的瓦片数据集，或直接连接Mapbox账号上传到用户自己的账户空间中。
- **carto**也是Mapbox维护的一个开源项目，它是将CartoCSS脚本解析成Mapnik XML地图样式的解释器，是用node.js开发的。CartoCSS的语言参考手册就在carto项目的wiki中。

我会尽力保证这份文档与官方英文文档同步。github仓库中保存了中文版文档的markdown格式版本，以及通过[Ulysses](http://www.ulyssesapp.com/)和[Marked 2](http://marked2app.com)生成的pdf与html版本。html的在线版本可以直接从[这里](http://luliu.me/projects/carto_zh-cn/)查看，但阅读体验并不好，所以建议阅读[gitbook上的在线版本](http://tumluliu.gitbooks.io/carto_zh-cn/)。

关于翻译的更多信息请参考这篇[文章](http://luliu.me/?p=40)，或关注[我的博客](http://luliu.me)了解最新的翻译整理进展。

### 翻译整理计划

《指南》最初只是一系列有关CartoCSS的文档的中文翻译。但因为这些文档之间本身并没有很强的逻辑关系，所以我还是把它们重新进行了组织，形成了目前的结构。但是这样的组织的结果就是在各个章节之间需要加入一些承上启下的衔接内容，以及在必要的时候需要对原文进行必要的补充和内容调整。基于这些考虑，翻译整理工作会按照以下步骤进行。

1. **原文整理**。将有必要列入书中的英文原文进行整理，以markdown格式放入对应章节。
2. **翻译**。以段为单位进行翻译。翻译的过程中保留原文，每段译文写在原文下面，翻译的过程尽量忠于原文，但鼓励意译。如果觉得原文阐述不清或有错误，需要修正的时候，请用“译注”标出，并尽可能提交issue给官方仓库，或在邮件列表中进行讨论。
3. **校对**。将译文进行整理，梳理语句、段落、章节，修订格式与错别字。
4. **增补**。在章节过渡、承上启下等位置和需要其它说明解释的地方补充文字。
5. **整体审校**。

因为都是利用业余时间在做这件事情，所以这些步骤暂时不设deadline，但大致的计划是在2015年4月1日前完成第一版。

### 致谢

感谢以下用户参与本书翻译：

- [Anran Yang](http://www.github.com/yarray)

特别感谢[Anran Yang](http://www.github.com/yarray)对本书进行封面设计。

### 源码仓库

本书内容以markdown格式保存在github上，仓库地址为：

[www.github.com/tumluliu/carto\_zh-cn](http://www.github.com/tumluliu/carto_zh-cn)

### 问题与反馈

如果对翻译有任何问题或建议请到项目的仓库中[提交issue](https://github.com/tumluliu/carto_zh-cn/issues)，或者直接与该项目的负责人[Lu Liu](http://www.github.com/@tumluliu)联系：

- 邮箱：nudtlliu#gmail.com
- 网站：[luliu.me](http://luliu.me)　
