---
title: 手把手教你用FineBI，快速搭建可视化大屏
date: 2017-12-28 16:20:50
categories: "开发"
tags:
	- 职场
	- 可视化

---

相信大家最近都看过许多关于大屏、管理驾驶舱的文章，有些驾驶舱数据分析页面确实是狂拽炫酷得不行，但是对于这些页面具体的制作过程，怎么样最佳展现数据的业务价值，想必大家还是有些云里雾里。

本篇文章以销售管理驾驶舱为例，给大家介绍一个完整的管理驾驶舱搭建实例，手把手教会你30分钟如何快速搭建BI销售管理驾驶舱。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI]

下面让我们从底层数据处理开始入手，在实际的业务数据分析场景中，往往需要利用到多张维度表和事实明细表中的数据（例如下图中的分公司维度表和合同事实表），而FineBI产品的敏捷数据模型是通过构建雪花型的数据模型，支持跨数据源关联以消除孤岛的企业数据应用痛点，较传统的针对具体业务进行sql表拼接、宽表拼接的星型数据模型，雪花型拥有着灵活、可复用性极高等诸多天然的优势。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 1]

我们搭建好上图的销售DEMO业务包的数据表和关联模型之后，下一步就可以进行正式的销售管理驾驶舱看板搭建了。

在给大家介绍具体制作过程之前先讲解一下通常管理驾驶舱的布局方式。管理驾驶舱往往展现的是一个企业全局的业务，一般分为主要指标和次要指标两个层次，主要指标反映核心业务，次要指标用于进一步阐述分析。所以在制作时给予不一样的侧重，这里推荐几种常见的版式。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 2]

上面几个版式不是金科定律，只是通常推荐的主次分布版式，能让信息一目了然。实际项目中，不一定使用主次分布，也可以使用平均分布，或者可以二者结合进行适当调整。比如下图所示，指标很多很多，存在多个层级的，就根据上面所说的基本原则进行一些微调，效果会很好。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 3]

回归到正题，看看我们是如何来进行销售管理驾驶舱的实际制作过程的。首先，既然是管理中心驾驶舱，那么我们可以先从企业领导和高层最为关注的公司签单金额和回款金额入手。对于这样的汇总指标，选择仪表板进行数据展示再合适不过了。选择拖入合同事实表中的合同金额和合同回款表中的回款金额两个指标，样式这里我们选择圆环仪表盘，同时两个指标的单位都设置成亿，最大刻度我们输入当前合同金额，2.78亿。这样一来，2.78亿的合同回款，2.25亿的回款金额以及80.87%的总的回款率也就统计出来了,企业的签单金额和回款金额/回款率都一目了然，这里的组件标题我们可以隐藏掉。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 4]

其次由于企业本身的客户遍布全国，那么用地图来做数据分析统计便再合适不过了。这里我们拉一个地图组件，然后把分公司信息表中的省份和合同信息表中的合同金额两个字段分别拖拽过来进行数据统计，合同金额的单位设置为万，图例设置居右，隐藏组件标题，这样全国各省份的销售合同分布情况通过地图得到了充分的展示。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 5]

通常领导和公司高层一般都比较关注公司每年的合同和汇款情况统计走势，这里我们选择折线图进行随时间变化的数据趋势分析展现。分类轴放入合同信息表中的签约年份字段，左值轴放入需要统计的合同金额和回款金额，同时我们可能希望还能看到每一年下面对应的月份的合同回款情况，在FineBI中只需要把签单月份放到分类轴之后，取消掉勾选显示即可实现从年份钻取到每个月份的功能。单位这边可以根据具体数量级选择，这里我们选择百万，同时隐藏掉网格线，组件命名为“年度签单&&回款统计”。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 6]

在企业中，往往财务统计是根据每年的季度进行计算的，这里我们再统计一下每年中每个季度的签单情况。组件选择堆积面积图，然后分类选择签约年份，系列选择季度，左值轴选择合同金额，样式设置中把单位修改为百万，同时隐藏掉网格线，组件命名为“季度签单堆积面积图”。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 7]

对于企业每年签的合同，领导可能关注合同金额区间段的一个分布情况，而实际合同金额在数据库的存储其实是离散的，但是这种数据自定义分组统计在FineBI产品中实现起来其实非常简单。这里我们选择饼图进行数据分布统计，首先把合同信息表中的合同金额拉到分类中，需要统计合同个数的话，只需要把合同信息记录数字段拉到指标栏同时计数依赖选择合同ID。对于合同金额我们这边进行区间分组方式，分组方式改变为自定义，然后分别按照一万以下、一万到十万、十万到三十万、三十万到五十万、五十万到一百万、100万以上进行合同金额大小的区间分类合同个数统计。从统计结果中我们可以看出，目前企业的主要签单金额是分布在十万到三十万之间，占比为31.74%。然后图表这边我们希望能够展示得更加美观一些，样式设置中饼图类型选择不等角玫瑰图，内径大小填70，图例居右，组件命名为“合同金额区间分布。”

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 8]

除了时间维度之外，因为有分公司，所以我们还可以更加直观的统计每个分公司以及每个客户经理对应的合同回款情况。首先在分类轴中放入省份、客户经理字段，客户经理字段取消勾选以实现能够钻取到每个省份下去查看对应的客户经理的回款情况。左值轴选择合同金额、回款金额，在右值轴中添加计算指标为“回款金额/合同金额”，回款率的图表类型选择折线图。样式设置中，左指轴设置为万，右值轴设置为百分比，图例居右，隐藏网格线，组件命名为“各地区签单回款情况统计。”

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 9]

在企业的销售签单统计中我们往往可能会比较关心回款金额前十名和后十名分别是哪些人，以及这些人的签单金额分别又是多少，对于销售靠前的经理人可进行销售经验分享，对于销售业绩落后的经理人及时督促。我们先来做回款前十名的销售分析，选择对比条形图，分类区域拖拽客户经理字段，值轴1选择合同金额，值轴2选择回款金额，然后对客户经理进行过滤，选择回款金额前N个，输入10确定即可。样式设置中我们把单位设置成万，隐藏网格线，组件命名为“销售回款前十名”。下面再来做回款后十名的销售分析，这里我们只需要复制“销售回款前十名”即可，然后重命名为“销售回款后十名”，进入详细设置界面，将对客户经理的原先的过滤条件修改为汇款金额的后N个，输入10即可。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 10]

再后面我们可能需要查看签单以及回款的明细数据，以进行每笔订单的详细数据查看。先选择明细表，依次拖入销售区域、客户经理、客户名称、合同签约时间、合同金额、回款金额，这里我们可以再添加一个“回款率”计算指标为“回款金额/合同金额”。对于回款率指标，修改数量级单位为百分比，同时我们可以做一个样式预警标识，对于回款率低于50%的的数据进行红色标记，大于50%的数据进行绿色标记。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 11]

同时明细表这边我们跟“各地区签单回款情况统计”组件做一个联动，这样一来，当我们点击各个地区或者钻取到客户经理的维度时，就能够在下方的明细表中轻松看到对应地区或者客户经理的明细数据了。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 12]

最后加上文本组件作为仪表板的标题，调节文字加粗、设置文字颜色，字体大小选择32号，背景设置透明，局样式中选择样式三，设置组件背景为透明，同时对于标题的两侧添加背景图片边框，以增加驾驶舱细节处的细节感官。

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 13]

布局方式改变为自由布局，地图组件以及其他组件的位置可参照示意图进行一下微调，微调完毕之后点击预览模板，这样我们的管理驾驶舱页面就制作完成了。通过这样的一个销售管理驾驶舱数据分析看板，领导和决策层想要看的相关企业销售数据都一览无余，童鞋们是不是很期待想尝试一下呢？

![手把手教你用FineBI，快速搭建可视化大屏][FineBI 14]


[FineBI]: /pro/os/crawler/2IJI-MBQJ-RZZV.jpg
[FineBI 1]: /pro/os/crawler/B3Q7-JAVI-NRJR.jpg
[FineBI 2]: /pro/os/crawler/2UJA-JRYA-ZRME.jpg
[FineBI 3]: /pro/os/crawler/ZBZQ-RJ3Q-NA6N.jpg
[FineBI 4]: /pro/os/crawler/JUBZ-AJVB-26BF.jpg
[FineBI 5]: /pro/os/crawler/2QIV-RZAI-MIF2.jpg
[FineBI 6]: /pro/os/crawler/UFAY-FBFE-6NUE.jpg
[FineBI 7]: /pro/os/crawler/I7BZ-JIYB-2EE3.jpg
[FineBI 8]: /pro/os/crawler/6VBY-JAEF-IBZN.jpg
[FineBI 9]: /pro/os/crawler/YVJA-BYIY-BYZV.jpg
[FineBI 10]: /pro/os/crawler/JZMI-NNNN-2YJI.jpg
[FineBI 11]: /pro/os/crawler/E3A6-3MYY-AY2Y.jpg
[FineBI 12]: /pro/os/crawler/NURV-EQ6N-VJIE.jpg
[FineBI 13]: /pro/os/crawler/QJRI-IN7N-IQVE.jpg
[FineBI 14]: /pro/os/crawler/BEAE-EAEQ-6ZUU.jpg
 *  **原文作者：** 数据分析不是个事儿
 *  **原文链接：** https://www.toutiao.com/item/6504510001405493773/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。