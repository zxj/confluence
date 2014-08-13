---
layout: default
title: ECMA5中严格模式的限制
---

- 限制意外的创建全局变量

> 不允许不使用var关键字声明变量

- 给变量赋值产生的错误将会明确抛出

> 如将NaN作为一个变量赋值时、给只读的属性、给不允许扩展的对象属性赋值时将会明确的抛出错误异常

- 通过delete删除不可删除的属性时将会明确抛出异常

> 如，通过delete删除通过var声明的变量

- 严格模式不允许存在重名的属性名称

{% highlight javascript %}
'use strict'
var o={p:1,p:2} //!!! syntax error
{% endhighlight %}

- 严格模式下不允许函数参数名称重名

{% highlight javascript %}
function sum(a,a,b){ //!!! syntax error
	'use strict'

	return a+b;
}
{% endhighlight %}

- 严格模式下不允许使用八进制

{% highlight javascript %}
'use strict'

var a=012; //!!! syntax error
{% endhighlight %}

- 严格模式下禁止使用with
- 不允许通过eval执行的js代码向外层作用域增加变量

> 如果在严格模式下执行eval，则eval的执行上下文也是以严格模式执行

- 禁止对eval和arguments赋值
- 不允许通过arguments[0]与函数的第一个参数进行双向绑定，即通过arguments[0]可以获取第一个参数arg1的值，但更改arg1的值不会反映到arguments[0]中。
- arguments.callee不再支持
- 禁止普通函数内部通过this引用全局对象
{% highlight javascript %}
//未使用严格模式
function fn(){
	return !this; //this指向window
}
fn() //返回false

function fn(){
	"use strict"

	return !this; //报错
}
{% endhighlight %}
- 函数声明必须在顶层
{% highlight javascript %}
"use strict";

if(true){
	function f(){} //报错
}
for(var i=0;i<10;i++){
	function fn(){} //报错
}
{% endhighlight %}