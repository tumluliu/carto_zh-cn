# CartoCSS中文指南

## 前言

这是一份根据[carto](https://github.com/mapbox/carto)的最新文档和[MapBox Studio的在线帮助文档](https://www.mapbox.com/mapbox-studio/style-quickstart/)综合整理翻译的中文版CartoCSS指南与语言参考手册。翻译的目的是为了方便越来越多的使用CartoCSS进行制图的中文用户了解这种制图样式语言中各种属性的含义及用法。读这份文档需要具有一些的地理信息系统（GIS）、地图制图学（Cartography）方面的背景知识。由于CartoCSS最终会被解译为[mapnik](https://github.com/mapnik/mapnik)样式并进行制图渲染，所以如果知道并了解mapnik的工作原理，那么会对CartoCSS的语法要素及工作机理有更深刻的理解，但这不是使用CartoCSS进行制图所必需的。关于背景知识，可以参考mapnik项目中的[相关文档](https://github.com/mapnik/mapnik/wiki/LearningMapnik)。

我会尽力保证这份文档与官方英文文档同步。github仓库中保存了中文版文档的markdown格式版本，以及通过[Ulysses](http://www.ulyssesapp.com/)和[Marked 2](http://marked2app.com)生成的pdf与html版本。html的在线版本可以直接从[这里](http://luliu.me/projects/carto_zh-cn/)查看，但阅读体验并不好，所以建议阅读[gitbook上的在线版本](http://tumluliu.gitbooks.io/carto_zh-cn/)。

关于翻译的更多信息请参考这篇[博客](http://luliu.me/?p=40)。

### 翻译整理计划

《指南》最初只是一系列有关CartoCSS的文档的中文翻译。但因为这些文档之间本身并没有很强的逻辑关系，所以我还是把它们重新进行了组织，形成了目前的结构。但是这样的组织的结果就是在各个章节之间需要加入一些承上启下的衔接内容，以及在必要的时候需要对原文进行必要的补充和内容调整。基于这些考虑，翻译整理工作会按照以下步骤进行。

1. **原文整理**。将有必要列入书中的英文原文进行整理，以markdown格式放入对应章节。
2. **翻译**。以段为单位进行翻译。翻译的过程中保留原文，每段译文写在原文下面，翻译的过程尽量忠于原文，但鼓励意译。如果觉得原文阐述不清或有错误，需要修正的时候，请用“译注”标出，并尽可能提交issue给官方仓库，或在邮件列表中进行讨论。
3. **校对**。将译文进行整理，梳理语句、段落、章节，修订格式与错别字。
4. **增补**。在章节过渡、承上启下等位置和需要其它说明解释的地方补充文字。
5. **整体审校**。

因为都是利用业余时间在做这件事情，所以这些步骤暂时不设deadline，但大致的计划是用两个月左右完成第一版。

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
- 网站：[luliu.me](luliu.me)　