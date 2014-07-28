---
layout: default
title: Javascript标准参考阅读笔记
---

##语法

###数据类型

- 六种数据类型，两个特殊类型
	> 数值型
	> 字符型
	> 布尔型
	> 函数型
	> Object
	> 数组型
	> null,undefined(两个特殊类型)

###Object

- delete只能删除对象本身的属性，不能删除继承的属性，也不能用来删除var声明的变量。
- 如果删除一个不存在的属性，不会报错，而且返回true。所以delete之后的属性能保证为undefined，但不能确保该属性是否存在过。