---
title: Javascript中按值和按引用的比较
date: 2012-12-15 23:06:45
categories: "开发"
tags:
	- Java

---

## 按值和按引用的比较 ##

Numbers 和 Boolean 类型的值 (**true** 和 **false**) 是按值来复制、传递和比较的。当按值复制或传递时，将在计算机内存中分配一块空间并将原值复制到其中。然后，即使更改原来的值，也不会影响所复制的值（反过来也一样），因为这两个值是独立的实体。

对象、数组以及函数是按引用来复制、传递和比较的。 当按地址复制或传递时，实际是创建一个指向原始项的指针，然后就像拷贝一样来使用该指针。如果随后更改原始项，则将同时更改原始项和复制项（反过来也一样）。实际上只有一个实体；“复本”并不是一个真正的复本，而只是该数据的又一个引用。

当按引用比较时，要想比较成功，两个变量必须参照完全相同的实体。例如，两个不同的 **Array** 对象即使包含相同的元素也将比较为不相等。要想比较成功，其中一个变量必须为另一个的参考。要想检查两个数组是否包含了相同的元素，比较 **toString()** 方法的结果。

最后，字符串是按引用复制和传递的，但是是按值来比较的。请注意，假如有两个 **String** 对象（用 **new** String("something") 创建的），按引用比较它们，但是，如果其中一个或者两者都是字符串值的话，按值比较它们。