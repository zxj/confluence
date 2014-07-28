---
layout: default
title: Javascript标准参考阅读笔记
---

##语法

**概述**

- 语句和表达式的区别是：语句没有返回值，表达式有返回值
- Javascript中所有数值内部都是以64位浮点数运算的，可表示的最大精确值是2的53次方


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

- try...catch...finally这三者之间的执行顺序
{% highlight javascript %}
function f() {
    try {
        console.log(0);
        throw "bug";
    } catch(e) {
        console.log(1);
        return true; // 这句会延迟到finally代码块结束再执行
        console.log(2); // 不会运行
    } finally {
        console.log(3);
        return false; // 这句会覆盖掉前面那句return
        console.log(4); // 不会运行
    }
    
    console.log(5); // 不会运行
}

var result = f(); 
// 0
// 1
// 3

result
// false
{% endhighlight %}

**字符串**

- Base64是一种将二进制转换为可打印字符的编码形式。在浏览器环境中提供了两个方法来处理Base64，btoa:将二进制或字符转换为Base64格式；atob：将Base64格式转换为二进制或字符串。这两个方法只能转换Ascii编码字符。超出Ascii范畴将会报错。若非要转换非Ascii码字符转换为Base64，必须在中间插入一个编码环节。
{% highlight javascript %}
window.btoa("Hello World")
// "SGVsbG8gV29ybGQ="

window.atob("SGVsbG8gV29ybGQ=")
// "Hello World"

window.btoa('你好')
// InvalidCharacterError: An invalid or illegal character was specified, such as in an XML name.

function b64Encode( str ) {
    return window.btoa(unescape(encodeURIComponent( str )));
}
 
function b64Decode( str ) {
    return decodeURIComponent(escape(window.atob( str )));
}

// 使用方法
b64Encode('你好') // "5L2g5aW9"
b64Decode('5L2g5aW9') // "你好"

{% endhighlight %}

**运算符**

- 加法与字符串连接运算符

{% highlight javascript %}

1+1 //2
1+"1" //11
1+1+"1" //21
"1"+1+1 //111
1-"1" //0
+"3" //3
-true //-1

{% endhighlight %}

- 求余运算符的正负，由第一个运算子的正负号决定

{% highlight javascript %}
//错误的实现
function isOdd(v){
	return v % 2==1;
}
isOdd(-5) //false
isOdd(-4) //false

//正确的实现
function isOdd(v){
	return Math.abs(v) % 2==1;
}
isOdd(-5) //true
isOdd(-4) //false

{% endhighlight %}

- 对象与原始类型比较时（这里指的是广义的对象，包括数值和函数），对象转换为原始类型的值再进行比较
- undefined和null与其他类型比较时都为false，他们相互比较的结果为true
- 相等运算符的缺点

{% highlight javascript %}
''=='0' //false
''==0 //true
'0'==0 //true

false=='false' //false
false=='0' //true
undefined==false //false
null==false //false
undefined==null //true

' \t\r\n '==0 //true
{% endhighlight %}

- 不需要引入临时变量的两值交换方式（连续对两个值进行三次异或运算(详见[维基百科](http://en.wikipedia.org/wiki/XOR_swap_algorithm)）

{% highlight javascript %}
var a=10,
	b=20;

a^=b;
b^=a;
a^=b;

a //20
b //10
{% endhighlight %}

- 位运算操作的是整数，返回的是32位整数值，最大值：2147483647
- 位运算的取整效果

> 位运算可以用来取整，因为位运算只对整数有效，遇到小数时会舍去，只保留整数部分。所以，将一个小数与0进行或运算，等同于对该数去除小数部分。

{% highlight javascript %}
2.9 | 0 //2
{% endhighlight %}
> 通过位运算实现的rgb2hex
{% highlight javascript %}
function rgb2hex(r,g,b){
	return '#'+((1<<24)+(r<<16)+(g<<8)+b).toString(16).substr(1);
}

rgb2hex(255,255,255) //#ffffff
{% endhighlight %}
- 圆括号的作用是求值，如果将语句放到圆括号中就会报错，因为语句没有返回值。

**数据类型转换**

***强制类型转换***

- 原始类型强制转换为Number类型

> 数值：转换后还是原来的值

> 字符串：如果可以被解析，则转换为相应的数值，否则为NaN，空字符串转换为0

> 布尔值：true为1，false为0

> undefined：转换为NaN

> null：转换为0

- 对象转换为Number类型

> 先调用自身的'valueOf()'方法，如果该方法返回原始值类型，则直接对该值使用Number方法，不再进行后续步骤；

> 如果'valueOf()'返回复合类型，再调用对象自身的'toString()'方法，如果'toString()'返回原始值类型，则对该值使用Number，不再进行后续步骤；

> 如果'toString()'返回的类型为复合类型，则报错。

{% highlight javascript %}
Number({a:1}) //NaN
{% endhighlight %}

> 上述代码的执行过程是，先调用对象的valueOf()，返回{a:1}，然后再调用toString，返回'[object Object]'，然后再进行Number()操作，则返回NaN。

- 原始类型强制转换为String类型

> 数值：转换为相应的字符串

> 字符串：转换后还是之前的字符串

> 布尔值：true为"true"，false为"false"

> undefined：转换为"undefined"

> null：转换为"null"

- 对象转换为String类型

> 先调用对象的'toString()'方法，如果返回的是原始类型，则直接应用String操作，不再进行后续步骤；

> 如果'toString()'返回的是复合类型，再调用'valueOf()'方法，如果'valueOf()'返回的是原始值，则直接应用String操作，不再进行后续步骤；

> 如果'valueOf()'返回的是复合类型，则直接报错。

- 原始类型转换为Boolean类型

> undefined、null、""、0、NaN转换后false，其他的原始值全部为true

- 对象转换为Boolean类型

> 所有对象的布尔值类型都为true

***自动转换***

- 当遇到以下几种情况，Javascript会自动转换数据类型

> 不同类型的数据进行相互运算；

> 对非布尔值类型的数据求布尔值；

> 对非数值类型的数据使用一元操作（+,-）

- 自动转换为字符串

> 字符串的自动转换主要发生在加法运算时，当一个值为字符串，另一个值为非字符串时，则后者自动转换为字符串

> 除加法操作可能把运算子转换为字符串，其他运算都会把两侧的运算子转换为数值

> 除了二元，一元运算符+、-也会把运算子转换为数值

{% highlight javascript %}
Number({a:1}) //NaN

var obj = {
    valueOf: function () {
            console.log("valueOf");
            return {};
    },
    toString: function () {
            console.log("toString");
            return {}; 
    }
};

Number(obj)
// TypeError: Cannot convert object to primitive value

Number({valueOf:function (){return 2;}})
// 2

Number({toString:function(){return 3;}})
// 3

Number({valueOf:function (){return 2;},toString:function(){return 3;}})
// 2

String(123) // "123"

String("abc") // "abc"

String(true) // "true"

String(undefined) // "undefined"

String(null) // "null"

String({a:1})
// "[object Object]"

String({a:1}.toString())
// "[object Object]"

var obj = {
    valueOf: function () {
            console.log("valueOf");
            return {}; 
    },
    toString: function () {
            console.log("toString");
            return {}; 
    }
};

String(obj)
// TypeError: Cannot convert object to primitive value

String({toString:function(){return 3;}})
// "3"

String({valueOf:function (){return 2;}})
// "[object Object]"

String({valueOf:function (){return 2;},toString:function(){return 3;}})
// "3"

Boolean(undefined) // false

Boolean(null) // false

Boolean(0) // false

Boolean(NaN) // false

Boolean('') // false

Boolean(new Boolean(false))
// true

'5' + 1 // '51'
'5' + true // "5true"
'5' + false // "5false"
'5' + {} // "5[object Object]"
'5' + [] // "5"
'5' + function (){} // "5function (){}"
'5' + undefined // "5undefined"
'5' + null // "5null"

'5' - '2' // 3
'5' * '2' // 10
true - 1  // 0
false - 1 // -1
'1' - 1   // 0
'5'*[]    // 0
false/'5' // 0
'abc'-1   // NaN

+'abc' // NaN
-'abc' // NaN
+true // 1
-false // 0

{% endhighlight %}
