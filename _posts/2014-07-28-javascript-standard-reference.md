---
layout: default
title: Javascript标准参考阅读笔记
---

##语法

**数据类型**

- 六种数据类型，两个特殊类型

> 数值型、字符型、布尔型、函数型、Object、数组型、null,undefined(两个特殊类型)

- null和undefined
> 两者都表示“空、不存在”的意思，从语义上来说：null表示空对象，用来表示对象；undefined表示变量未定义，用来表示变量。
> 转换为'布尔类型'后，两者是等价的

{% highlight javascript %}

null==undefined //true

{% endhighlight %}

> undefined和null与数值进行操作时行为不同

{% highlight javascript %}

console.log(undefined+5) //输出NaN

console.log(null+5) //输出5

{% endhighlight %}



**Object**

- delete只能删除对象本身的属性，不能删除继承的属性，也不能用来删除var声明的变量。
- 如果删除一个不存在的属性，不会报错，而且返回true。所以delete之后的属性能保证为undefined，但不能确保该属性是否存在过。
- in 运算符对数组同样有效，对继承的属性判断同样有效，该运算符只能用来验证'可枚举'属性。
- with语句块中对某个变量读写时，如果该变量存在于with打开的对象，则是对对象的属性的读写；如果不存在，则是对全局变量的读写。

**数值**

- isNaN返回值为true时，并不能表示传递的参数一定是NaN，isNaN("string")得返回值也是NaN，NaN
是Javascript种唯一和自身不相等的值。
- Infinity可以参与布尔运算，Infinity是Javascript最大的值；-Infinity是Javascript中最小的值
- parseFloat会先解析科学技术法然后再进行转换
{% highlight javascript %}
parseFloat("0.00314E+3") //3.14
{% endhighlight %}
- parseFloat和Number的区别
{% highlight javascript %}
parseFloat(true) //NaN
Number(true) //1

parseFloat("") //NaN
Number("") //0

parseFloat(null) //NaN
Number(null) //0

parseFloat("3.14#") //3.14
Number("3.14") //NaN
{% endhighlight %}

**错误**