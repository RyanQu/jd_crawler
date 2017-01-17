# jd_crawler
Author: RyQ
Date: 2017/01/16
Ver: 0.9

##项目介绍
###一些不太相关的话

作为此次项目整理的第一个项目，我选择了这个 Python 爬虫。一方面是因为需要一些一手的数据为以后的数据分析使用，另一方面则是来自工作坊大一小朋友们的压力。

在此之前我还真的没有用 Python 写过爬虫，之前所有的爬虫都是用 Java 写的。结果现在大一学生就掌握这项技巧了，自己也应该补上这一课，毕竟做 Data Science 的，一出门大家都是聊 Python。在此前我非常的不喜欢这门语言，虽然大家都说它优雅，做完这个爬虫，我也还是不喜欢，之后的爬虫或者做分布式可能还是以 Java 为主。

这个项目长期以来在我的 WorkSpace 里的名字叫做 Gxy_crawler，其实算是个人帮葛同学的一个小忙，帮人 debug，因为要整理上传 GitHub 的关系，所以整个代码都重构了，完全按照自己的思路来写。

本来想做的很完善的，结果还是有一些虎头蛇尾，很多功能、抛出机制还有缓存处理的一些坑其实都留好了，但是懒得写了，明天开学，还有很多项目等着我去填坑。

###关于项目
爬取京东网站上的相关数据，原始 URL: [https://list.jd.com/list.html?cat=737,13297,1300&ev=%402047_15280&go=0&JL=3_产品类型_烟灶套装#J_crumbsBa](https://list.jd.com/list.html?cat=737,13297,1300&ev=%402047_15280&go=0&JL=3_产品类型_烟灶套装#J_crumbsBa)

这是一个商品类别筛选的 list，内容是烟灶套装。

说实话我真的很懵逼，葛同学你研究这个玩意干啥……你看看别人的京东爬虫项目，先不说内容质量，光看名字就觉得很有意思。[京东百万记录分析中国人罩杯分布](https://zhuanlan.zhihu.com/p/23790374)

抓取的主要数据：（都是针对于商品的，以下用“A商品”简化表达）

1. A商品名称
2. A商品在筛选中的排名
3. A商品的价格
4. A商品的评论

##项目依赖

只用到了一些Python的很基础的库

1. 获取链接： `requests` `urllib` `urllib2`
2. 字符处理： `re` `json` `BeautifulSoup`
3. 系统设置： `sys` `time` `random`

##京东反爬虫

###URL缺少参数

原始URL：[https://list.jd.com/list.html?cat=737,13297,1300&ev=%402047_15280&go=0&JL=3_产品类型_烟灶套装#J_crumbsBa](https://list.jd.com/list.html?cat=737,13297,1300&ev=%402047_15280&go=0&JL=3_产品类型_烟灶套装#J_crumbsBa)

在原始URL中，没有关于页面的参数，更讨厌的是为什么还有中文，我对 `Python` 的编码问题真的是头疼，这个事情一会儿会说到。

###循环页面
这个是帮人 debug 的核心内容，当时就是因为这个问题解决不了我才出手的。

葛同学搜了搜资料，小试牛刀，兴奋的抓取到了京东的 `html` 文件，存储在了本地，命名为 `index.html`。

然而循环了一会发现问题出现了，第6页的内容与第1页的完全一样，第11页的内容与第一页一样……以此类推。

如果通过浏览器访问，显然是不会出现这个问题的，所以问题出现在了http请求的 `head` 上面。根据后面的尝试发现，设置 `head` 的时候一定要把浏览器中显示的http请求完整的复制下来，只有 `User-Agent` 是不够的，`Cookies` 也是一个非常关键的点。当更换开发调试环境或者浏览器环境发生变化的时候，需要更新`Cookies`，否则依然会出现上面的问题。 

###


抓取后的数据样式如 `data-set.csv` 文件所示，时间和运算资源有限，暂时只抓了前100多条尝了尝咸淡。