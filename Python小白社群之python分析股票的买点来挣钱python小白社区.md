---
title: Python小白社群之python分析股票的买点来挣钱
date: 2017-11-06 06:53:22
categories: "开发"
tags:
	- 投资
	- 编程语言
	- 财经
	- Python

---

python是个好工具。可以用来作很多事情，当然，也可以用来分析股票。

现在我们对科大讯飞的股票用python做个可视化分析，分析它的买点。或者说，什么情况发生了，股票会上涨。我们

只要在之前买入，不就赚钱了吗。哈哈

先上程序

![Python小白社群之python分析股票的买点来挣钱][Python_python]

python程序分析科大讯飞股票

程序20行意思是股票的ma5,ma20,close指标。close是收盘价。具体这个指标什么意思呢，见下面的解释。

    MA是“移动平均线”的简称，后面的数字：5、10、20.....是时间周期。移动平均线，Moving Average，简称MA，原本的意思是移动平均，由于将其制作成线形，所以一般称之为移动平均线，简称均线。它是将某一段时间的收盘价之和除以该周期。 比如日线MA5指5天内的收盘价除以5 。移动平均线是由著名的美国投资专家Joseph E.Granville（葛兰碧，又译为格兰威尔）于20世纪中期提出来的。均线理论是当今应用最普遍的技术指标之一，它帮助交易者确认现有趋势、判断将出现的趋势、发现过度延生即将反转的趋势。常用移动平均线有5天、10天、30天、60天、120天和240天的指标。其中，5天和10天的短期移动平均线，是短线操作的参照指标，称做日均线指标；30天和60天的是中期均线指标，称做季均线指标；120天、240天的是长期均线指标，称做年均线指标。

好，我们上图，大家先看看。

![Python小白社群之python分析股票的买点来挣钱][Python_python 1]

科大讯飞的收盘价和ma5,ma20

同学们，看出来了吗？有什么规律？  


还没看出来？

好吧，python大大给大家指点下，放大图。

![Python小白社群之python分析股票的买点来挣钱][Python_python 2]

大图解释python分析出来的买点

如大图，给大家点了红点。大家可以看到，ma5,ma20相交的位置，就是红点位置。然后绿线会发生什么事？  


对了，上涨，如图可见，ma5,ma20相交后，绿线会上涨。my god!是不是相交时，我们买入，就可以妥妥的挣钱呢？

至少这个图是这么说的，但是股市套路多，稍后大家还会学到python分析其他方面的，防止我们被套路。

每天十分钟，python改变生活。我是python小白社群的python大大，例子里的数据和程序源码有兴趣的都可以联系我索取。  



[Python_python]: /pro/os/crawler/ARAB-RBYU-2ERJ.jpg
[Python_python 1]: /pro/os/crawler/IV6F-AIVY-AIVU.jpg
[Python_python 2]: /pro/os/crawler/ZIAF-ZVRU-UEE2.jpg
 *  **原文作者：** python小白社区
 *  **原文链接：** https://www.toutiao.com/item/6484957557101167118/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。