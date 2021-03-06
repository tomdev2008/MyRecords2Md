---
title: sql分库、分表
date: 2014-01-12 15:58:09
categories: "开发"
tags:
	- sql

---

### 1. 分表的分类 ###

#### 1）纵向分表 ####

将本来可以在同一个表的内容，人为划分为多个表。（所谓的本来，是指按照关系型数据库的第三范式要求，是应该在同一个表的。）

分表理由：根据数据的活跃度进行分离，（因为不同活跃的数据，处理方式是不同的）

案例：

对于一个博客系统，文章标题，作者，分类，创建时间等，是变化频率慢，查询次数多，而且最好有很好的实时性的数据，我们把它叫做冷数据。而博客的浏览量，回复数等，类似的统计信息，或者别的变化频率比较高的数据，我们把它叫做活跃数据。所以，在进行数据库结构设计的时候，就应该考虑分表，首先是 纵向分表的处理。

这样 纵向分表后：

首先存储引擎的使用不同，冷数据使用MyIsam 可以有更好的查询数据。活跃数据，可以使用Innodb ,可以有更好的更新速度。

其次，对冷数据进行更多的从库配置，因为更多的操作是查询，这样来加快查询速度。对热数据，可以相对有更多的主库的横向分表处理。

其实，对于一些特殊的活跃数据，也可以考虑使用memcache ,redis之类的缓存，等累计到一定量再去更新数据库。或者mongodb一类的nosql 数据库，这里只是举例，就先不说这个。

#### 2）横向分表 ####

字面意思，就可以看出来，是把大的表结构，横向切割为同样结构的不同表，如，用户信息表，user\_1,user\_2 等。表结构是完全一样，但是，根据某些特定的规则来划分的表，如根据用户ID来取模划分。

分表理由：根据数据量的规模来划分，保证单表的容量不会太大，从而来保证单表的查询等处理能力。

案例：同上面的例子，博客系统。当博客的量达到很大时候，就应该采取横向分割来降低每个单表的压力，来提升性能。例如博客的冷数据表，假如分为100个表，当同时有100万个用户在浏览时，如果是单表的话，会进行100万次请求，而现在分表后，就可能是每个表进行1万个数据的请求（因为，不可能绝对的平均，只是假设），这样压力就降低了很多很多。

### 2. 分库 ###

分数据库设计，将可能从压力性能上会提升几个档次，当然单次执行效率不会比单数据库来的高的，毕竟存在着数据库切换的效率问题。分库以及主从数据库搭 配是可以比较好改善数据库并发瓶颈的方案。**原则：大数据量，分库；大访问量，主从。很多时候，都是这两者并行 ( 本文不讨论 cache) 。** 
我想，如果要实现分库以及主从关系，那么数据库服务器数量将是非常可观，在应用程序中随时切换到某一台服务器，将是非常头痛的问题，配置更换，变量名称，是不是会有一大堆呢？如何寻找更好的解决方案将是本文谈论的话题。
首先是分库使得数据库颇多的问题。什么情况下分库？或许有些人还搞不明白为什么要分库，我就简要说一下自己的经验猜测。比如一个博客程序，一般设计是 将日志存放在一张日志表中。假设是一个多用户博客，那么将会关联一个 uid ，如果数据量不大，这样设计是没有问题的，但是当日志量巨大，一天有几十万条日 志记录录入的时候，而且访问量也比较可观的时候，我想不可能每个用户来访问日志列表，都去从这包含几千万条日志记录的数据表中去找那么几条，效率可见一 斑。这个时候就该考虑到分库的问题。**如何分？有一个很简单的分表方法，即，根据 uid 段，将日志记录在各个数据库中，当然，这个分布还是需要根据以往统计 结果做出调整的，因为用户日志分布肯定不是均匀的。设置好 uid 段，然后根据 uid 索引到指定数据库配置，创建一个数据库对象即可。配置信息可能如下：**
Copy code
$configs\['db\_info'\]\['blog'\]\[0\] = array(
'db\_host' => '192.168.0.1',
'db\_name' => 'blog',
'db\_user' => 'root',
'db\_pass' => '',
);
$configs\['db\_info'\]\['blog'\]\[1\] = array(
'db\_host' => '192.168.0.2',
'db\_name' => 'blog',
'db\_user' => 'root',
'db\_pass' => '',
);
$configs\['db\_info'\]\['blog'\]\[2\] = array(
'db\_host' => '192.168.0.2',
'db\_name' => 'blog',
'db\_user' => 'root',
'db\_pass' => '',
);
//...and so on

<!-- \[if !supportEmptyParas\]--> <!-- \[endif\]-->

<!-- \[if !supportEmptyParas\]--> <!-- \[endif\]-->

<!-- \[if !supportEmptyParas\]--> <!-- \[endif\]-->

垂直分库指的是根据应用来分数据库，比如博客一个数据库，论坛一个数据库。水平分库是指，根据某些规则，将同一个应用 / 表的数据分布在不同的库上。比如根据用户 ID 把用户的博客文章分布在 5 个数据库上。


\--部分转:http://blog.csdn.net/zhangbonefu/article/details/8899628