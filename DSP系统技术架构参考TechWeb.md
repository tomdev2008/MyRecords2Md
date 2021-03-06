---
title: DSP系统技术架构参考
date: 2017-09-06 15:21:43
categories: "开发"
tags:
	- NoSQL
	- 人工智能
	- Redis
	- 大数据
	- 设计模式

---

前面基本已经将DSP的典型模式、主要机制及要点都介绍了。可能有些同学会好奇DSP系统内部的技术架构。下面截取部分DSP系统的技术架构图供大家参考，同样对于非技术的同学对此有个感性认识即可。也不做大篇幅的展开了。

1. 技术架构概要

如图7-22所示，DSP系统从技术架构上涉及：投放平台、投放设置用户交互模块（setup UI）、报表（Report）、算法引擎等等模块。算法引擎模块主要是大数据及算法的机器学习大量采用分布式技术（例如hadoop），对用户日志、人群数据进行建模及机器智能处理。算法引擎模块处理好的人群数据、算法模型等等数据通过海量内存技术（例如redis）暂存在内存中，便于Bid投放引擎快速查询使用，全部暂存在内存中的目的是为了在100ms完成竞价过程，确保在DSP方<30ms处理完成，为网络通讯流出时间。Bid投放引擎是典型的大集群模式用于响应大并发的请求，且确保每个请求<30ms处理完成。Bid投放引擎的投放规则（预算、频次、投放策略设置等等数据）也都是存在内存中的便于快速查询。投放策略设置的数据内容都是用户通过投放设置用户交互模块中的界面完成的。另外还有一些十分重要的辅助模块，例如：广告曝光点击数据回收模块、idmapping模块、大数据报表模块、内置DMP模块等等。

![DSP系统技术架构参考][DSP]

图7-22 技术架构概要示例

2. DSP内部技术处理流程概要

DSP内部技术处理主要依赖一些关键技术处理设施，主要的包括：原始海量log系统、海量消息并行处理队列（例如采用spark技术）、海量内存系统（例如采用redis技术）、业务系统关系型数据数据库等等。如图7-23所示，一条技术处理线路是广告请求处理线：广告竞价Bidder海量的实时广告请求处理会产生海量的原始log、同时Bidder也频繁的同海量内存系统交互读写广告请求相关的频次、消耗等等数据，然后广告请求log经过过并行处理队列处理灌入报表数据库及相应的大数据人群及模型数据库。另一条技术处理线路是广告曝光、点击等等监测数据的回收，开始也是产生大量的原始log、同时数据回收引擎同海量内存系统交互写如广告曝光、点击相关的频次、消耗等等数据。然后广告曝光、点击log经过过并行处理队列处理灌入相应报表数据库及相应的大数据人群及模型数据库，同时并行处理队列进行大量的机器智能分析更新部分人群数据及模型数据，同时同步更新到Bidder数据库及内容系统中供Bidder实时竞价时使用。

![DSP系统技术架构参考][DSP 1]

图7-23 DSP内部技术处理流程概要示例

3. DSP竞价核心处理流程概要（<30ms）

如图7-24所示，DSP的Bidder竞价模块设计约束核心处理时间极短，<30ms。为了解决适配不同ADX流量的不同接口。在接受广告请求，及输出返回时，会针对不同ADX平台接口使用适配器设计模式采用不同适配器予以处理。但整体处理流程不变。中间业务处理部分也使用过滤器的设计模式，增加新业务时根据业务需要增加过滤器实现即可。这样做的好处是整体的Bidder竞价核心模块处理流程框架相对稳定，不会随这业务的变化而变化。具备十分强大的业务灵活性和应对高性能的水平扩充性。

![DSP系统技术架构参考][DSP 2]

图7-24 DSP竞价核心处理流程概要示例

4. 竞价程序处理过程概要

如图7-25所示，Bidder竞价处理器内部也会依据业务处理依次分为：索引器快速过滤广告（采用索引器的好处是检索效率极高，当然索引器仅能用户简单的过滤条件，例如：尺寸索引、平台及广告位索引、浏览器索引、操作系统索引、地域索引等等）。广告过滤（投放策略相关规则需计算的过滤条件是无法使用索引器，例如：预算、曝光、日期、频次、人群定向、创意类型等等）。上述这两层过滤都是为了广告请求过滤可供投放的候选广告列表，然后通过出价算法的处理给出该广告列表中各广告的出价（这里可能会用到动态出价算法，也可能使用的固定出价策略（采用何种出价策略及是否使用算法都是在投放设置界面中有人工设设置的））。然后会进行低价过滤（根据广告请求中的底价过滤掉出价低于底价的那些候选广告）。最后排序并决策胜出（根据各候选广告的出价及算法附带给出的优先级权重综合排序，排名第一的胜出，即将以该广告内容准备竞价返回）。曝光点击动态代码生成（以上一步胜出的广告内容生成曝光点击动态代码，生成动态曝光点击代码有很多目的，例如防作弊，全程携带投放参数追踪等等）。Bid/Unbid日志记录（结束处理时异步启动）。

![DSP系统技术架构参考][DSP 3]

图7-25竞价程序处理过程概要示例

5. 分布式集群概要

如图7-26所示，为了应对海量的广告竞价业务需要，及大数据的分布式计算基础设施的需要。DSP在系统架构设计上需要支持分布式支持水平扩容，架构支持大并发、大数据、高可用、高容错等等特征。

![DSP系统技术架构参考][DSP 4]

图7-26 分布式集群概要示例

文字表现力有限，欢迎参加《9月9日“DSP上”专题【线下大课堂】》面对面为您答疑解惑讲透您关心的问题。现场福利：可以为您的新书定制个性的签名题词。

网上文章较零散，若想系统化学习，可考虑考虑作者刚出版的原版新书《程序化广告实战》，各大电商网站（如：京东）均有售。


[DSP]: /pro/os/crawler/JVBM-UZYI-3UBM.jpg
[DSP 1]: /pro/os/crawler/ZJBI-AJEU-EQ63.jpg
[DSP 2]: /pro/os/crawler/ABBA-U22A-VEZV.jpg
[DSP 3]: /pro/os/crawler/6ZN6-FZBQ-AEUV.jpg
[DSP 4]: /pro/os/crawler/E3UY-3MBE-YRUV.jpg
 *  **原文作者：** TechWeb
 *  **原文链接：** https://www.toutiao.com/item/6462562347184554509/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。