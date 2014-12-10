# CartoCSS中文指南

## 前言

这是一份根据[carto](https://github.com/mapbox/carto)的最新文档和[MapBox Studio的在线帮助文档](https://www.mapbox.com/mapbox-studio/style-quickstart/)综合整理翻译的中文版CartoCSS指南与语言参考手册。翻译的目的是为了方便越来越多的使用CartoCSS进行制图的中文用户了解这种制图样式语言中各种属性的含义及用法。读这份文档需要具有一些的地理信息系统（GIS）、地图制图学（Cartography）方面的背景知识。由于CartoCSS最终会被解译为[mapnik](https://github.com/mapnik/mapnik)样式并进行制图渲染，所以如果知道并了解mapnik的工作原理，那么会对CartoCSS的语法要素及工作机理有更深刻的理解，但这不是使用CartoCSS进行制图所必需的。关于背景知识，可以参考mapnik项目中的[相关文档](https://github.com/mapnik/mapnik/wiki/LearningMapnik)。

我会尽力保证这份文档与官方英文文档同步。这个仓库中保存了中文版文档的markdown格式版本，以及通过[Ulysses](http://www.ulyssesapp.com/)和[Marked 2]生成的pdf与html版本。html的在线版本可以直接从[这里](http://luliu.me/projects/carto_zh-cn/)查看。

关于翻译的更多信息请参考这篇[博客](http://luliu.me/?p=40)。

### 更新

### 致谢

感谢以下用户参与本书翻译：

- [Anran Yang](http://www.github.com/yarray)

特别感谢[Anran Yang](http://www.github.com/yarray)对本书进行封面设计。

### 源码仓库

本书内容以markdown格式保存在github上，仓库地址为：

[www.github.com/tumluliu/carto\_zh-cn](http://www.github.com/tumluliu/carto_zh-cn)

### 问题与反馈

如果对翻译有任何问题或建议请到项目的仓库中提交issue，或者直接与Lu Liu联系：

- 邮箱：nudtlliu#gmail.com
- 网站：luliu.me　