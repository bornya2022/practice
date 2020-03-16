# JavaScript学习笔记

## 一.JavaScript基础

> JavaScript，通常缩写为 JS，是一种高级的，解释执行的编程语言。JavaScript 是一门基于原型、函数先行的语言，是一门多范式的语言，它支持面向对象编程，命令式编程，以及函数式编程。它提供语法来操控文本、数组、日期以及正则表达式等，不支持 I/O，比如网络、存储和图形等，但这些都可以由它的宿主环境提供支持。它已经由 ECMA（欧洲计算机制造商协会）通过 ECMAScript 实现语言的标准化。它被世界上的绝大多数网站所使用，也被世界主流浏览器（Chrome、IE、Firefox、Safari、Opera）支持。
>
> 虽然 JavaScript 与 Java 这门语言不管是在名字上，或是在语法上都有很多相似性，但这两门编程语言从设计之初就有很大的不同，JavaScript 的语言设计主要受到了 Self（一种基于原型的编程语言）和 Scheme（一门函数式编程语言）的影响。在语法结构上它又与C语言有很多相似（例如 if 条件语句、while 循环、switch 语句、do-while 循环等）。
>
> 在客户端，JavaScript 在传统意义上被实现为一种解释语言，但在最近，它已经可以被即时编译（JIT）执行。随着最新的 HTML5 和 CSS3 语言标准的推行它还可用于游戏、桌面和移动应用程序的开发和在服务器端网络环境运行，如 Node.js。

注：定义来自于维基百科。

**JavaScript 的组成**

- ECMAScript：JavaScript 的语法标准。
- DOM：JavaScript 操作网页上的元素的 API。
- BOM：JavaScript 操作浏览器的部分功能的 API。

**JavaScript 的特点**

- 可以使用任何文本编辑工具编写，然后使用浏览器就可以执行程序。
- 是一种解释型脚本语言：代码不进行预编译，从上往下逐行执行，不需要进行严格的变量声明。
- 主要用来向 HTML 页面添加交互行为。

### 1.1js实例

新建一个 test.html 文件(后续的例子中，将不再提醒建立 test.html 文件，大家根据个人需求自行创建对应的 html 文件，完成后续操作)：

> 由于实验楼使用是 WebIDE 的在线环境，所以有不熟悉对同学请阅读下：[实验楼 WebIDE 使用指南](https://www.shiyanlou.com/library/shiyanlou-docs/feature/webide)，`前端开发`部分的内容。

首先来看看范例代码：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <script>
            console.log("hello world");
        </script>
    </body>
</html>
```

上述代码的含义就是在我们的控制台打印一句话：hello world。首先教大家怎么查看：将上述代码复制到一个 html 文件中，然后在浏览器中运行，点击 F12，再点击控制台上的 Console，即可查看。如下图所示：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547012930726.png/wm)

值得注意的是我们可以直接在控制台上输入 JavaScript 代码，然后点击 enter 让其执行。如下图所示我们执行几行简单的加减乘除：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547013173817.png/wm)

通过上面的代码，我们可以看出 JavaScript 代码是放在<script> ……</script> 标签里，而包含 JavaScript 代码的 script 标签，我们可以放在 <body> ……</body>标签里，也可以放在<head> ……</head>标签里。比如上述范例也可以这样写：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <script>
            console.log("hello world");
        </script>
    </head>
    <body>    
    </body>
</html>
```

执行结果没有什么区别，不同的是执行顺序，简单的来说，放在前面的会先执行。此外，和 CSS 引入相类似，JavaScript 也可以通过外部引入。首先我们需要创建一个扩展名为 .js 的文件，然后在 html 页面中引入它，同样的拿上述范例来修改，我们首先创建一个叫 test.js (名字自己取，但是扩展名一定要是 .js,只有这样才能够识别 JavaScript 代码)的文件，然后在里面写上我们的 JavaScript 代码：

```html
console.log("hello world");
```

在 html 文件中写上如下代码：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <script src="test.js">

        </script>
    </body>
</html>
```

值得注意的是 test.js 文件要和你的 html 文件在同一目录下才能用上面的方式引用，否则的话需要使用相对路径还是绝对路径来引入 js 文件，就需要根据实际情况灵活运用了。前两种方式都是直接把 JavaScript 代码放在 HTML 中，在页面加载的同时，那些 JavaScript 的代码就被解析了。而把 JavaScript 代码放在外部文件中，只有在事件被触发，需要该段 JavaScript 代码时，才调用执行。这样做有个好处，当页面比较复杂的时候，把大量的 JavaScript 代码放到外部文件，只有在需要的时候才执行，那么会明显地加快页面加载速度，而且实现结构化分离，也便于我们维护自己的代码，所以建议大家养成外部引入的方式来写我们的 JavaScript 代码。

### 1.2变量

#### 1.2.1变量是什么

在计算机中，数据都存在内存中。而一个变量，就是一个用于存放数值的容器，每个变量存放的数值是可变的，每个变量都有其独有的名字，每个变量都占有一段内存。

注：变量不是数值本身，变量仅仅是一个用于储存数值的容器。

#### 1.2.2声明变量

通过 var 关键字来声明变量，比如：

```javascript
var name = "实验楼";
```

上述代码声明了一个名为 name 的变量，并赋值为“实验楼”，注意此处的等于符号（=）为赋值符号，不是我们传统意义上理解的等号。

变量的命名规则如下：

- 变量名必须以字符或下划线“_”开头，不能以数字开头
- 变量可以包含数字、从 A 至 Z 的大小字母
- 不能使用 JavaScript 中的关键字做变量名
- 变量名不能有空格
- 严格区分大小写，比如：name 和 Name 就是两个完全不同的变量。

另外在 JavaScript 中，变量也可以不作声明，而在使用时再根据数据的类型来确其变量的类型，如：

```javascript
x = 50 ;     // 变量 x 为整数
```

#### 1.2.3变量类型

- Number:你可以在变量中存储数字，不论这些数字是像 10（整数）这样，或者像 3.1415926（浮点数）。

```javascript
var x1 = 10;
var x2 = 3.1415926;
```

- String:存储字符（比如 "shiyanlou"）的变量，字符串可以是引号中的任意文本，你可以使用单引号或双引号，也可以在字符串中使用引号，只要不匹配包围字符串的引号即可：

```javascript
var carname="shiyanlou";
var carname='shiyanlou';
var answer="I Love 'shiyanlou'";
var answer='I Love "shiyanlou"';
```

- Boolean:布尔类型的值有两种：true 和 false。通常被用于在适当的代码之后，测试条件是否成立，后续会讲到。
- Array：数组是一个单个对象，其中包含很多值，方括号括起来，并用逗号分隔。后续我们将会对数值进行详细的讲解，此处看两个简单的数值例子：

```javascript
var myNameArray = ['Tom', 'Bob', 'Jim'];
var myNumberArray = [10,15,20];
```

- Object:对象类型。同样的我们会在后续的课程中详细讲解什么是对象，此处先看一个简单的例子：

```javascript
var student = { name : 'Tom', age : 18 };
```

#### 1.2.4动态类型

JavaScript 是一种“动态类型语言”，这意味着不同于其他一些语言(如 C、Java)，你不需要指定变量将包含什么数据类型（例如 number 或 string）,通通用 var 关键字声明就是了。比如如果你声明一个变量并给它一个带引号的值，浏览器就会知道它是一个字符串：

```javascript
var myString = 'Hello';
```

值得注意的是就是引号中是一个数字，它依然是 string 类型的。我们可以在控制台中通过 typeof 函数，来查看我们声明的变量是什么类型的。

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547018886902.png/wm)

#### 1.2.5注释

单行注释：用来描述下面一个或多行代码的作用。单行注释快捷键：Ctrl + /

```javascript
//这是一个变量
var name = "zhangsan";
```

多行注释：用来注释多条代码。多行注释快捷键：Ctrl + Shift + /

```javascript
/*
var name = "zhangsan";
var age = 18;
console.log(name, age);
*/
```

### 1.3数字和运算符

#### 1.3.1数字类型

- 整数。例如：1,2,100，-10。
- 浮点数:就是小数。例如：0.2，3.1415926。
- 双精度：是一种特定类型的浮点数，它们具有比标准浮点数更高的精度。

注：我们这里作为了解就可以了。因为和其他语言不同的是，在 JavaScript 中，不管什么数字类型的通通用 var 声明就可以了，而且 JavaScript 中只有一个数据类型:Number。

#### 1.3.2算数运算符

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547022268324.png/wm)

加减乘除的运算和我们平时所用的差不多，大家可以自行在控制台中尝试。这里需要提醒大家的是累加运算和累减运算的用法:

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547022963371.png/wm)

从上述的图片中可以看出我们不能将这些直接应用于一个数字，不是对值进行操作，而是为变量赋值一个新的更新值。num++ 其实是 num = num + 1 的省略写法。从图片上来看可能不好理解 num++ 和 ++num 的区别。我们来写一个例子：

```javascript
var num1 = 3;
var result1 = num1++;
console.log(result1);
console.log(num1);
var num2 = 4;
var result2 = ++num2;
console.log(result2);
console.log(num2);
```

最后控制台打印的数字为：3 4 5 5。这里我们也可以看出 num++ 和 ++num 的区别，前者是先赋值再自增，后者是先自增再赋值。同样的 num-- 和 --num 也是一样的效果，大家可以自行尝试一下。

#### 1.3.3操作运算符

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547024191664.png/wm)

#### 1.3.4比较运算符

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547024508797.png/wm)

在控制台中输入这些示例，如果成立的话会返回 true 如果不成立的话则返回 false。

#### 1.3.5逻辑运算符

![img](https://doc.shiyanlou.com/document-uid18510labid147timestamp1515116985209.png/wm)

#### 1.3.6运算符的优先级

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547261443500.png/wm)

### 1.4数组

通过我们前面所学的我们可以知道如果我们要保存一个数据，我们可以用 var 关键字去定义一个变量，但是如果数据很多呢？难道我需要一个个去定义？比如一个班上很多人，我们 var name1 = "张三";var name2 = "李四";...一直这样定义下去？显然这是不科学的，而这也由此引出了我们的数组。首先我们来看看维基百科对于数组的定义：

> 在计算机科学中，数组数据结构（英语：array data structure），简称数组（英语：Array），是由相同类型的元素（element）的集合所组成的数据结构，分配一块连续的内存来存储。利用元素的索引（index）可以计算出该元素对应的存储地址。

最简单的数据结构类型是一维数组。例如，索引为 0 到 9 的 32 位整数数组，可作为在存储器地址2000，2004，2008，...2036中，存储10个变量，因此索引为 i 的元素即在存储器中的 2000+4×i 地址。数组第一个元素的存储器地址称为第一地址或基础地址。

#### 1.4.2数组语法

```javascript
var myarray = new Array(1,2,3,4,5); // 创建数组同时赋值

//or

var myarray = [1,2,3,4,5]; //直接输入一个数组（称 “字面量数组”）
```

注：我们可以用上述两种方法创建数组，myarray 指的是定义的数组名，可以自己取，数组储存的的数据可以是任何类型（数字，字符，布尔值等）。每一个值都有一个索引值，从 0 开始。比如：

```javascript
var color = ["red","green","blue","yellow"];
color[0]; // returns "red"
color[1]; // returns "green"
color[2]; // returns "blue"
color[3]; // returns "yellow"
color[4]; // returns undefined
```

#### 1.4.2多维数组

多维数组就是数组中还包含数组，我们可以一层一层的来看。比如：

```javascript
var student = [["张三","男","18"],["李四","女","20"]];
student[0][2]; // returns "18"
```

#### 1.4.3操作数组

**修改数组**

修改数组中的元素内容也很简单，直接为它提供新值就可以了。比如：

```javascript
var color = ["red","green","blue","yellow"];
color[0] = "black";
color; // returns  ["black", "green", "blue", "yellow"]
```

**获取数组长度**

同样的我们使用 length 来获取数组的长度。比如：

```javascript
var color = ["red","green","blue","yellow"];
color.length; // returns 4
```

**数组和字符串之间的转换**

通过 split() 方法，将字符串转换为数组。比如：

```javascript
"1:2:3:4".split(":")    // returns ["1", "2", "3", "4"]
"|a|b|c".split("|")    // returns ["", "a", "b", "c"]
```

相反的我们通过 join() 方法将数组转换为字符串。比如：

```javascript
["1", "2", "3", "4"].join(":"); // returns "1:2:3:4"
["", "a", "b", "c"].join("|"); // returns "|a|b|c"
```

注：我们同样可以使用 toString() 方法将数组转换为字符串，但是 join() 方法可以指定不同的分隔符，而 toString() 方法只能是逗号。

**添加和删除数组项**

在数组尾部添加一个或多个元素，使用 push() 方法。比如：

```javascript
var arr = ["1", "2", "3", "4"];
arr.push("5","6");
arr; // returns ["1", "2", "3", "4", "5", "6"]
```

使用 pop() 方法将删除数组的最后一个元素，把数组长度减 1，并且返回它删除的元素的值。如果数组已经为空，则 pop() 不改变数组，然后返回 undefined 值。比如：

```javascript
var arr = ["1", "2", "3", "4"];
arr.pop(); //returns 4
arr; // returns ["1", "2", "3"]
var arr1 = [];
arr1.pop(); //returns undefined
arr1; //returns []
```

unshift() 和 shift() 从功能上与 push() 和 pop() 完全相同，只是它们分别作用于数组的开始，而不是结尾。大家可以直接在控制台自行尝试一番。

注：数组的方法其实还有很多，我们将在后续的课程中一一为大家讲解。

### 1.5 Null 和 Undefined

null 和 undefined 都表示无,但是也有一些区别。现在控制台上执行以下语句：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547019784085.png/wm)

这里需要说明的是：== 是相等操作符,比较值是否相等，如果相等输出为 true，否则为 false。=== 是全等操作符,比较值和类型是否都相等，如果都相等输出为 true，否则为 false。通过我们在控制台中的实践可以发现，null 和 undefined 的值不等于 0，它们的值相等，但是类型不相等。undefined 表示所有没有赋值变量的默认值，而 null 则表示一个变量不再指向任何对象地址。

### 1.6字符串

在前面的变量章节中，我们已经简单讲过字符串的基础知识，这里我们再拓展一下。我们前面讲过我们可以使用单引号或双引号，也可以在字符串中使用引号，只要不匹配包围字符串的引号即可。比如：

```javascript
var carname= "shiyanlou";
var carname= 'shiyanlou';
var answer= "I Love 'shiyanlou'";
var answer= 'I Love "shiyanlou"';
```

下面的代码将会出现错误，因为它会混淆浏览器和字符串的结束位置:

```javascript
var x1 = 'I've got no right to take my place...';
```

聪明的你可能会觉得这样不行，我们就换种方法嘛，比如：

```javascript
var x1 = 'I have got no right to take my place...';
```

或者：

```javascript
var x1 = "I've got no right to take my place...";
```

没错这样做都是可行的方法，但是其实我们还有另外一种方法，使用转义符号。转义字符意味着我们对它们做一些事情，以确保它们在文本中被认可，而不是代码的一部分。在 JavaScript 中，我们通过在字符之前放一个反斜杠来实现这一点。试试这个:

```javascript
var x1 = 'I\'ve got no right to take my place...';
```

常用的转义符：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547519780285.png/wm)

**连接字符串**

我们通过 “+” 连接字符串，比如在控制台中输入下面的代码：

```javascript
var one = "Hello,jack.";
var two = "I'm rose";
result = one + two;
```

最后控制台显示结果为："Hello,jack.I'm rose"。

如果我们在控制台输入的是 "jack"+18 思考一下会报错吗？要不再自己动手实践看看结果？结果是：浏览器很聪明的把数字转换成了字符串，最后结果为"jack18"。另外输入 "20"+"19"，最后的结果也是字符串"2019"。

**字符串转换**

我们可以通过 toString 方法把数字转换成字符串。

```javascript
var myNum = 123;
var myString = myNum.toString();
typeof myString;
```

也可以通过 Number 对象把传递给它的字符串类型的数字转换为数字。

```javascript
var myString = '123';
var myNum = Number(myString);
typeof myNum;
```

注：如果传递的不是纯数字的字符串，则返回的不是数字，而是 NaN（not a number）。

**获取字符串的长度**

通过 length 属性获取字符串的长度,结果返回一个数字。比如：

```javascript
var myString = "hello world ";
myString.length;
```

上述代码在控制台中运行的结果为：12（除了字母还有两个空格）。

如果你要查找这个字符串的第一个字符，你可以这么做：

```javascript
myString[0];
```

同样的你也可以查找到其他的字符，比如第五个字符，就是 myString[4]，这是因为电脑是从 0 开始，而不是 1，因此我们都要执行减一操作。

**在字符串中查找子字符串并提取它**

1. 有时候我们需要判断一个较长的字符串里面是否存在一个我们指定的较小的字符串，就比如我们要查找一段话里面是否包含一个词或者一个字，这个时候我们可以使用 indexof() 方法来完成，更详细的语法为：

```javascript
str.indexOf(searchValue, fromIndex);
```

str 指的是我们需要查的较长的字符串，searchValue 表示我们指定的较小的字符串，fromIndex 表示调用该方法的字符串中开始查找的位置，是一个可选的任意整数值，也可以不写，默认是 0 表示从头开始查，fromIndex < 0 和 fromIndex = 0 是一样的效果，表示从头开始查找整个字符串。如果 fromIndex >= str.length,则该方法的返回值为 -1。这里有个特殊的情况：就是如果被查找的字符串（searchValue）是一个空字符串，那么当 fromIndex <= 0 时返回 0，0 < fromIndex <= str.length 时返回 fromIndex，fromIndex > str.length 时返回 str.length。这样说你可能不太明白，我们来实践一下看看实际效果：

```javascript
"Blue Sky".indexOf("Blue");     // returns  0
"Blue Sky".indexOf("Ble");    // returns -1
"Blue Sky".indexOf("Sky", 0); // returns  5
"Blue Sky".indexOf("Sky", -1); // returns  5
"Blue Sky".indexOf("Sky", 5); // returns  5
"Blue Sky".indexOf("Sky", 9); // returns -1
"Blue Sky".indexOf("", 0);      // returns  0
"Blue Sky".indexOf("", 5);     // returns 5
"Blue Sky".indexOf("", 9);     // returns 8
```

注：返回值指的是指定值的第一次出现的索引，如果没有找到 -1。indexOf 方法区分大小写，比如：

```javascript
"Blue Sky".indexOf("blue"); // returns -1
"Blue Sky".indexOf("Blue"); // returns 0
```

1. 当你知道字符串中的子字符串开始的位置，以及想要结束的字符时，slice()方法可以用来提取 它。比如：

```javascript
"Blue Sky".slice(0,3); // returns "Blu"
```

注：slice(strat，end)，第一个参数 start 是开始提取的字符位置，第二个参数 end 是提取的最后一个字符的后一个位置。所以提取从第一个位置开始，直到但不包括最后一个位置。另外第二个参数也可以不写，不写代表某个字符之后提取字符串中的所有剩余字符。比如：

```javascript
"Blue Sky".slice(2); // returns "ue Sky"
```

**转换大小写**

字符串方法 toLowerCase() 和 toUpperCase() 字符串并将所有字符分别转换为小写或大写。比如：

```javascript
var string = "I like study";
string.toLowerCase(); // returns "i like study"
string.toUpperCase(); // returns "I LIKE STUDY"
```

**替换字符串的某部分**

可以使用 replace() 方法将字符串中的一个子字符串替换为另一个子字符串。比如：

```javascript
var string = "I like study";
string.replace("study","sleep"); // returns "I like sleep"
```

注：字符串的操作方法其实还有很多，我们将在后续的课程中再为大家作深入讲解。

### 1.7类型转换

**转换成字符串**

1. 几乎每个值都有 toString() 方法。比如在控制台输入以下代码：

```javascript
var num = 8;
var numString = num.toString();
numString;  // returns "8"
var result = true;
var resultString = result.toString();
resultString;// returns "true"
```

数值类型的 toString() ,可以携带一个参数,输出对应进制的值。比如：

```javascript
var num = 16;
console.log(num.toString());  //"16" 默认是10进制
console.log(num.toString(10));//"16"
console.log(num.toString(2)); //"10000"
console.log(num.toString(8)); //"20"
console.log(num.toString(16));//"10"
```

1. String() 函数。比如在控制台中输入以下代码：

```javascript
var num1 = 8;
var num2 = String(num1);
num2; // returns "8"
var result = true;
var result1 = String(result);
result1; // returns "true"
```

注：因为有的值没有 toString() 方法，所以需要用 String(),比如 null 和 undefined。如下所示：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547257971275.png/wm)

1. 使用拼接字符串。比如在控制台中输入以下代码：

```javascript
var num1 = 16;
var num2 = num1 + "";
num2; // returns "16"
```

**转换成数值类型**

1. Number()可以把任意值转换成数值，如果要转换的字符串中有一个不是数值的字符，返回 NaN（not a number）。比如在控制台中依次输入以下代码：

```javascript
var num1 = Number(true); 
num1; // true 返回 1，false 返回 0

var num2 = Number(undefined);
num2;  // 返回 NaN

var num3 = Number(null);
num3; //返回 0

var num4 = Number("syl");
num4;  // 返回 NaN

var num5 = Number("   ");
num5; // 如果是空字符串返回 0

var num6 = Number(123);
num6; // 返回123，如果是数字型的字符，返回数字

var num7 = Number("123abc");
num7;  // 返回 NaN
```

1. parseInt() 把字符串转换成整数。比如在控制台中依次输入以下代码：

```javascript
var num1 = parseInt("12.3abc");  
num1; //返回 12，如果第一个字符是数字会解析知道遇到非数字结束,只取整，不是约等于

var num2 = parseInt("abc123");  
num2; //返回 NaN，如果第一个字符不是数字或者符号就返回 NaN

var num3 = parseInt(""); 
num3;       // 空字符串返回 NaN，但是 Number("")返回 0

var num4 = parseInt("520");
num4;      //返回 520

var num5 = parseInt("0xA"); 
num5;  //返回 10
```

另外值得注意的是，parseInt()可以传递两个参数，第一个参数是要转换的字符串，第二个参数是要转换的进制。大家可以自行尝试一下。

1. parseFloat()把字符串转换成浮点数。写法和parseInt()相似，主要有以下几个不同点：

- parseFloat不支持第二个参数，只能解析 10 进制数
- 如果解析的内容里只有整数，解析成整数

例子：

```javascript
parseFloat("10"); // returns 10
parseFloat("10.000"); // returns 10
parseFloat("10.01"); // returns 10.01
parseFloat(" 10 "); // returns 10
parseFloat("10 hours"); // returns 10
parseFloat("aaa 10 "); // returns NaN
```

1. 执行减 0 操作。比如：

```javascript
var n="10";
var m=n-0;
m; // returns 10
```

值得注意的是，如果该字符串不是纯粹的数字字符串，那么它执行减 0 操作后，虽然变成了一个数字类型，但是返回值为 NaN。比如：

```javascript
var n = "abc";
var m = n - 0;
m; // returns NaN
typeof(m); // returns "number"
```

**转换成布尔类型**

1. 使用 Boolean() 函数。比如：

```javascript
Boolean(123);// returns true
Boolean("abc"); // returns true
Boolean(null); // returns false
Boolean(undefined); // returns false
Boolean(NaN); // returns false
Boolean(0); // returns false
Boolean(""); // returns false
Boolean(false); // returns false
Boolean("false"); // returns true
Boolean(-1); // returns true
```

1. 流程控制语句会把后面的值隐式转换为布尔类型。比如：

```javascript
var message;
if(message) { //会自动把message转换成false,最后打印结果为:我很好
    console.log("你好啊");
} else{    
console.log("我很好");
}
```

## 二.JavaScript关键特性

### 2.1条件

生活中，条件与我们息息相关。举几个例子：如果这周放假，那么我就要出去玩。如果明天不下雨，我就和小花出去踢足球。如果我饿了，我要么吃饭，要么吃面，要么就忍着。同样的在 JavaScript 中，我们也有条件语句,下面为大家讲解在 JavaScript 中如何使用条件语句。

**if...else 语句**

1. 最基本的 if...else 语句。它的语法为：

```javascript
if(条件) {
    //当条件为 true 时执行的语句
} else {
    //当条件为 false 时执行的语句
}
```

例子：

```javascript
if(3>2) {
    console.log("我真帅");
} else {
    console.log("不可能");
}
```

上述例子在控制台中打印的语句为：我真帅。

1. if...else 嵌套。它的语法是：

```javascript
if (条件 1){
  //当条件 1 为 true 时执行的代码;
  }
else if (条件 2){
  //当条件 2 为 true 时执行的代码;
  }
else{
  //当条件 1 和 条件 2 都不为 true 时执行的代码;
  }
```

注：根据实际情况，还可以嵌套更多的 else if 。

例子：

```javascript
var d = new Date().getDay();
if( d==0) {
    console.log("今天星期天");
} else if(d==1) {
    console.log("今天星期一");
} else if(d==2) {
    console.log("今天星期二");
} else {
    console.log("好多啊，我不想写了");
}
```

**switch case 语句**

从前面的例子中我们可以看出来，当条件很多的时候，一直嵌套 else if 语句，显然是有点不科学的，由此我们引出了 switch case 语句，先来看看它的语法：

```javascript
switch(k)
{
case 1:
  执行代码块 1 ;
  break;
case 2:
  执行代码块 2 ;
  break;
default:
  默认执行（k 值没有在 case 中找到匹配时）;
}
```

通过 switch case 语句来改写上面的例子：

```javascript
var d = new Date().getDay();
switch(d) {
    case 0:
        console.log("今天星期天");
        break;
    case 1:
        console.log("今天星期一");
        break;
    case 2:
        console.log("今天星期二");
        break;
    case 3:
        console.log("今天星期三");
        break;
    case 4:
        console.log("今天星期四");
        break;
    case 5:
        console.log("今天星期五");
        break;
    default:
        console.log("今天星期六");
        break;
}
```

**三元运算符**

语法：

```shell
语法：条件表达式?结果1:结果2
```

注：含义：问号前面的位置是判断的条件，判断结果为 boolean 型，为 true 时执行结果 1，为 false 时执行结果 2。

例子:

```javascript
3>2?console.log("3比2大"):console.log("3比2小");
```

### 2.2循环

其实前面讲 switch case 语句的例子的时候，大家可能当时觉得是要比嵌套的 else if 语句要简单些，但是大家有没有想过如果不止这么多呢？有成千上万个数据呢？用 else if 嵌套或者 switch case 来写显然都是不科学的，而这也由此引出了我们接下来要学习的：循环。通过循环函数来帮助我们完成一些重复性的工作。

**for 循环**

for 循环是我们编码中经常会使用到的。先来看看它的语法结构：

```javascript
for (初始化;条件;增量){
         循环代码;
}
```

举个打印 1 到 100 的例子：

```javascript
for(var i=1;i<=100;i++){
    console.log(i);
}
```

**使用 break 跳出循环**

我们在前面的 switch case 结构中已经见过 break 语句了,当 switch 语句中符合输入表达式的情况满足时，break 语句立即退出 switch 语句并移动到代码之后。上述的 for 循环例子，我们来加个条件，使其能打印一个能被 7 整除的整数。

```javascript
for(var i=1;i<=100;i++){
    if(i%7==0){
        console.log(i);
        break;
    }
}
```

**使用 continue 跳过迭代**

使用 continue 跳过迭代，不是完全跳出循环，而是跳过当前循环而执行下一个循环。比如我们使用 continue 可以实现打印 1 到 100 所有能被 7 整除的整数，而前面的例子中只能打印出：7。

```javascript
for(var i=1;i<=100;i++){
    if(i%7==0){
        console.log(i);
        continue;
    }
}
```

这么写可能不好理解，大家或许会想到上面的例子不写 continue 最后打印的效果和写了是一样的啊，那 continue 是不是就没用了？其实 continue 主要是跳过当前循环去执行下一个循环也就是说，当前循环下的其他语句就不执行了。来看下面的例子：

```javascript
for(var i=1;i<=7;i++){
    if(i%7==0){
        console.log(i);
        continue;
        console.log("*");
    }
}
```

大家可以把上面的代码运行一下，然后把 continue 删除，再运行一下，细细体会一下 continue 语句的作用。

**while 语句 和 do while 语句**

在 JavaScript 中不止有 for 循环，还有其他的循环语句。我们先来看看 while 循环的语法结构：

```javascript
while (条件){
  //需要执行的代码;
  }
```

同样的，我们来写一个打印 1 到 100 之间的整数的例子：

```javascript
var i = 1;
while(i<=100){
    console.log(i);
    i++;
}
```

do while 循环的语法结构：

```javascript
do
  {
  需要执行的代码;
  }
while (条件)
```

例子：

```javascript
var i = 1;
do{
    console.log(i);
    i++;
}
while(i<=100)
```

注：而这两者的区别是，do while 循环在检测条件之前就会执行，也就是说，即使条件为 false，do while 也会执行一次循环代码。而 while 循环只有在条件为真的时候才执行。 可以这样简单记忆：while 循环，先判断再执行。do while 循环先执行一次再判断。

### 2.3函数

函数就是可以重复执行的代码块。我们平时写代码的时候，也会经常遇到重复使用的代码，那我们是重复的一行行敲，或者 cv（ctrl c + ctrl v）大法一顿操作呢？显然这样有点不科学，如果我们使用函数将这些重复的代码封装起来，那么在需要使用的时候，就可以直接调用了。

**浏览器内置函数**

其实在前面学习字符串和数组的时候，我们不知不觉已经使用了很多次函数了。比如我们通过 join() 方法将数组转换为字符串，通过 split() 方法，将字符串转换为数组。这些都是浏览器已经封装好的内置函数，而我们可以直接调用。严格来说,内置浏览器函数并不是函数，它们是方法，但是没关系，我们前期学习阶段可以暂时不管这些，函数和方法在很大程度上是可互换的。

#### 2.3.1 函数创建

**函数声明创建函数**

语法为：

```javascript
function functionName(parameters) {
  //执行的代码
}
```

例子：

```javascript
function f(a,b){
    console.log(a+b);
} //创建一个名为f的函数，它有两个形参a，b
f(2,3); //调用函数f，传入实参2和3，最终运行结果为在控制台上打印出5
```

**函数表达式创建函数**

JavaScript 函数可以通过一个表达式定义。函数表达式可以存储在变量中。

语法为：

```javascript
var functionName = function (parameters){
    //执行的代码  
}
```

把函数声明创建函数的例子改写为用函数表达式创建函数：

```javascript
var f = function(a,b){
    console.log(a+b);
}
f(2,3);
```

**函数声明和函数表达式的区别**

- 函数声明

```javascript
 //此处的代码执行没有问题，JavaScript解析器首先会把当前作用域的函数声明提前到整个作用域的最前面。
    f(2,3);
    function f(a,b) {
        console.log(a+b);
    }
```

- 函数表达式

```javascript
//报错：f is not a function
//这是因为函数表达式，如同定义其它基本类型的变量一样，只在执行到某一句时也会对其进行解析

    f(2,3);
    var f = function(a,b){
    console.log(a+b);
    }
```

**函数的参数**

- 形参：function f(a,b){return a+b;} //a,b 是形参，占位用，函数定义时形参无值。
- 实参：当我们调用上面的函数时比如 f(2,3);其中 2 和 3 就是实参，会传递给 a 和 b,最后函数中执行的语句就变成了：return 2+3; 。

注：在 JavaScript 中，实参个数和形参个数可以不相等。

**在 JavaScript 中没有重载**

```javascript
function f(a,b) {
       return a + b;
}
function f(a,b,c) {
       return a + b + c;
}
var result = f(5,6); 
result;// returns NaN
```

上述代码中三个参数的 f 把两个参数的 f 覆盖，调用的是三个参数的 f，最后执行结果为 NaN。而不是其他语言中的 5。

**在 JavaScript 中函数的返回值**

- 如果函数中没有 return 语句，那么函数默认的返回值是：undefined。
- 如果函数中有 return 语句，那么跟着 return 后面的值就是函数的返回值。
- 如果函数中有 return 语句，但是 return 后面没有任何值，那么函数的返回值也是：undefined。
- 函数在执行 return 语句后会停止并立即退出，也就是说 return 语句执行之后，剩下的代码都不会再执行了。
- 当函数外部需要使用函数内部的值的时候,我们不能直接给予，需要通过 return 返回。比如：

```javascript
var f = function(a,b){
    a+b;
}
console.log(f(2,3)); // 结果为undefined
var f = function(a,b){
    return a+b;
}
console.log(f(2,3)); // 结果为5。
```

**匿名函数**

匿名函数就是没有命名的函数，一般用在绑定事件的时候。语法为：

```javascript
function(){

}
```

例子：

```javascript
var myButton = document.querySelector('button');

myButton.onclick = function() {
  alert('hello');
}
```

注：将匿名函数分配为变量的值，也就是我们前面所讲的函数表达式创建函数。一般来说，创建功能，我们使用函数声明来创建函数。使用匿名函数来运行负载的代码以响应事件触发（如点击按钮） ，使用事件处理程序。

**自调用函数**

匿名函数不能通过直接调用来执行，因此可以通过匿名函数的自调用的方式来执行。比如：

```javascript
(function () {
  alert('hello');
})();
```

## 三.JSON

### 3.1JSON基础

> JSON（JavaScript Object Notation，JavaScript 对象表示法）是一种由道格拉斯·克罗克福特构想和设计、轻量级的数据交换语言，该语言以易于让人阅读的文字为基础，用来传输由属性值或者序列性的值组成的数据对象。尽管 JSON 是 JavaScript 的一个子集，但 JSON 是独立于语言的文本格式，并且采用了类似于 C 语言家族的一些习惯。
>
> JSON 数据格式与语言无关，脱胎于 JavaScript，但当前很多编程语言都支持 JSON 格式数据的生成和解析。JSON 的官方 MIME 类型是 application/json，文件扩展名是 .json。

注：定义来自维基百科。

下面给出两个简单的 json 示例：

```json
 {
    "name" : "zhangsan",
    "age" : 18,
    "gender" : "male"
}
{
    "students": [
        { "firstName":"san" , "lastName":"zhang" },
        { "firstName":"si" , "lastName":"li" },
        { "firstName":"wu" , "lastName":"wang" }
    ]
}
```

特别需要注意的是：

- JSON 是一种纯数据格式,它只包含属性,没有方法。
- JSON 的属性必须通过双引号引起来。
- JSON 要求有两头的 { } 来使其合法。
- 可以把 JavaScript 对象原原本本的写入 JSON 数据，比如：字符串，数字，数组，布尔还有其它的字面值对象。

### 3.2常用内置对象

前面在我们讲数组的方法和字符串的操作的时候，已经讲解了一部分内置对象的内容。本节，我们再来系统的学习一下一些常用的内置对象。

#### Array 对象

1. Array 对象的常用属性：length。获取数组的长度。
2. Array 对象的常用方法：

- concat()方法用于连接两个或多个数组，并返回结果。语法为：

```json
arrayObject.concat(arrayX,arrayX,......,arrayX);
```

例子：

```javascript
var a = [1,2,3];
var b = [4,5,6];
var c =["one","two","three"];
console.log(a.concat(b,c)); //打印结果为：[1, 2, 3, 4, 5, 6, "one", "two", "three"]
```

- join() 方法，将数组转换成字符串。（数组章节有详细介绍，这里不过多的赘述，下面的类似情况同样处理，大家看到这个方法，首先回想一下我们前面所学的知识，如有遗忘，再回去看一看加深记忆）。
- pop() 方法，删除并返回数组的最后一个元素。
- push() 方法，向数组的末尾添加一个或更多元素，并返回新的长度。
- reverse() 方法，颠倒数组的顺序。比如：

```javascript
var a = [1,2,3,4];
a.reverse();
console.log(a); // a 数组变成：[4, 3, 2, 1]。
```

- shift() 方法，删除并返回数组的第一个元素。
- unshift() 方法，向数组的开头添加一个或更多元素，并返回新的长度。
- slice() 方法，从某个已有的数组返回选定的元素。语法为：

```javascript
arrayObject.slice(start,end);
//strat 值是必需的，规定从何处开始选取。
//end 值可选，规定从何处结束选取，如果没有设置，默认为从 start 开始选取到数组后面的所有元素。
```

在控制台中输入以下代码：

```javascript
var a = [1,2,3,4,5,6];
a.slice(2,5); //结果为[3,4,5]，另外需要注意的是该方法不会修改数组，只是返回一个子数组，a 数组还是 [1,2,3,4,5,6]。
```

- splice() 方法，删除或替换当前数组的某些项目。语法为：

```javascript
arrayObject.splice(start,deleteCount,options)
// start 值是必需的，规定删除或替换项目的位置
// deleteCount 值是必需的，规定要删除的项目数量，如果设置为 0，则不会删除项目。
// options 值是可选的，规定要替换的新项目
// 和 slice() 方法不同的是 splice() 方法会修改数组。
```

在控制台中输入以下代码：

```javascript
var a = [1,2,3,4,5,6];
a.splice(2,2,"abc");
a; // 最终 a 数组变成了[1, 2, "abc", 5, 6]
```

- sort() 方法，将数组进行排序。语法为：

```javascript
arrayObject.sort(sortby);
// sortby 是可选的，规定排序顺序，必需是函数。如果没有参数的话，将会按照字母顺序进行排序，更准确的说是按照字符编码（可自行百度了解）的顺序进行排序。如果想按照其他标准进行排序，则需要提供比较函数。
```

例子：

```javascript
<script>
    var arr1 = ["a","z","k","w","x"];
    document.write(arr1 + "<br />");
    document.write(arr1.sort()+ "<br />" + "<br />");

    var arr2 = [11,55,22,44,66,33];
    document.write(arr2 + "<br />");
    document.write(arr2.sort() + "<br />" + "<br />");

    var arr3 = [1,22,44,6,55,5,2,4,66];
    document.write(arr3 + "<br />");
    document.write(arr3.sort() + "<br />" + "<br />");

    function sortNum1(a,b){
        return a - b;//从小到大排序
    }
    var arr4 = [1,22,44,6,55,5,2,4,66];
    document.write(arr4 + "<br />");
    document.write(arr4.sort(sortNum1) + "<br />" + "<br />");

    function sortNum2(a,b){
        return b - a;//从大到小排序
    }
    var arr4 = [1,22,44,6,55,5,2,4,66];
    document.write(arr4 + "<br />");
    document.write(arr4.sort(sortNum2) + "<br />" + "<br />");
</script>
```

运行结果为：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547275126629.png/wm)

- toString() 方法，把数组转换为字符串，并返回结果。

#### String 对象

1. String 对象的常用属性：length。获取字符串的长度。
2. String 对象的常用方法()：

- charAt() 方法，获取指定位置处字符。语法为：

```javascript
stringObject.charAt(index);
//字符串中第一个字符的下标是 0。如果参数 index 不在 0 与 string.length 之间，该方法将返回一个空字符串。
```

例子：

```javascript
var str = "Hello world!";
document.write(str.charAt(2));
//以上代码输出为 l
```

- charCodeAt() 方法，获取指定位置处字符的 Unicode 编码。语法为：

```javascript
stringObject.charCodeAt(index);
//字符串中第一个字符的下标是 0。如果 index 是负数，或大于等于字符串的长度，则 charCodeAt() 返回 NaN。
```

例子：

```javascript
var str = "Hello world!";
document.write(str.charCodeAt(2));
//以上代码输出为 l08
```

- concat() 方法，连接字符串,等效于 “+”，“+” 更常用。与数组中的 concat() 方法相似。
- slice() 方法，提取字符串的片断，并在新的字符串中返回被提取的部分（字符串章节有详细介绍，这里不过多的赘述，下面的类似情况同样处理）。
- indexOf() 方法，检索字符串。
- toString() 方法，返回字符串。
- toLowerCase() 方法，把字符串转换为小写。
- toUpperCase() 方法，把字符串转换为大写。
- replace() 方法，替换字符串中的某部分。
- split() 方法，把字符串分割为字符串数组。

#### Date 对象

Date 对象方法：

- Date()：返回当日的日期和时间。（输出是中国标准时间）
- getDate()：从 Date 对象返回一个月中的某一天 (1 ~ 31)。
- getDay()：从 Date 对象返回一周中的某一天 (0 ~ 6)。
- getMonth():从 Date 对象返回月份 (0 ~ 11)。
- getFullYear():从 Date 对象以四位数字返回年份。
- getHours():返回 Date 对象的小时 (0 ~ 23)。
- getMinutes():返回 Date 对象的分钟 (0 ~ 59)。
- getSeconds():返回 Date 对象的秒数 (0 ~ 59)。
- getMilliseconds():返回 Date 对象的毫秒(0 ~ 999)。

#### Math 对象

1. Math 对象的常用属性：

- E ：返回常数 e (2.718281828...)。
- LN2 ：返回 2 的自然对数 (ln 2)。
- LN10 ：返回 10 的自然对数 (ln 10)。
- LOG2E ：返回以 2 为底的 e 的对数 (log2e)。
- LOG10E ：返回以 10 为底的 e 的对数 (log10e)。
- PI ：返回π（3.1415926535...)。
- SQRT1_2 ：返回 1/2 的平方根。
- SQRT2 ：返回 2 的平方根。

1. Math 对象的常用方法：

- abs(x) ：返回 x 的绝对值。
- round(x) ：返回 x 四舍五入后的值。
- sqrt(x) ：返回 x 的平方根。
- ceil(x) ：返回大于等于 x 的最小整数。
- floor(x) ：返回小于等于 x 的最大整数。
- sin(x) ：返回 x 的正弦。
- cos(x) ：返回 x 的余弦。
- tan(x) ：返回 x 的正切。
- acos(x) ：返回 x 的反余弦值（余弦值等于 x 的角度），用弧度表示。
- asin(x) ：返回 x 的反正弦值。
- atan(x) ：返回 x 的反正切值。
- exp(x) ：返回 e 的 x 次幂 (e^x)。
- pow(n, m) ：返回 n 的 m 次幂 (nm)。
- log(x) ：返回 x 的自然对数 (ln x)。
- max(a, b) ：返回 a, b 中较大的数。
- min(a, b) ：返回 a, b 中较小的数。
- random() ：返回大于 0 小于 1 的一个随机数。

### 3.3对象的创建和访问

创建对象的方式有很多，下面为大家讲解几种常用的方式。

#### 通过对象字面量来创建

```javascript
var student = {
  name: 'zhangsan',
  age: 18,
  gender : 'male',
  sayHi: function () {
    console.log("hi,my name is "+this.name);
  }
}; 
```

把上面的代码复制到控制台中，然后尝试依次输入下面的代码看看效果：

```javascript
student.name;
student.age;
student.gender; //调用对象的属性
student.sayHi(); //调用对象的方法
```

通过上面的例子你会发现对象的属性和方法通过点来访问。

#### 通过 new Object() 创建对象

```javascript
var student = new Object();
  student.name = 'zhangsan',
  student.age = 18,
  student.gender = 'male',
  student.sayHi = function () {
    console.log("hi,my name is "+this.name);
  }
```

#### 通过工厂函数创建对象

```javascript
function createStudent(name, age, gender) {
  var student = new Object();
  student.name = name;
  student.age = age;
  student.gender = gender;
  student.sayHi = function(){
    console.log("hi,my name is "+this.name);
  }
  return student;
}
var s1 = createStudent('zhangsan', 18, 'male');
```

#### 自定义构造函数

```javascript
function Student(name,age,gender){
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.sayHi = function(){
       console.log("hi,my name is "+this.name);
  }
}
var s1 = new Student('zhangsan', 18, 'male');
```

#### new 关键字

构造函数，是一种特殊的函数。主要用来在创建对象时初始化对象，即为对象成员变量赋初始值，总与 new 运算符一起使用在创建对象的语句中。这里有需要特别注意的几点：

- 构造函数用于创建一类对象，首字母要大写。
- 内部使用 this 关键字给对象添加成员。
- 使用 new 关键字调用对象构造函数。

#### this 详解

在 JavaScript 中，我们经常会使用到 this 关键字，那么 this 到底指向什么呢？这里有一个口诀：谁调用 this，它就是谁。大家也可以从前面的例子中细细体会一下。

- 函数在定义的时候 this 是不确定的，只有在调用的时候才可以确定
- 一般函数直接执行，内部 this 指向全局 window。比如：

```javascript
function test() {
    console.log(this);
}
test();  //window.test();
```

- 函数作为一个对象的方法，被该对象所调用，那么 this 指向的是该对象。
- 构造函数中的 this，始终是 new 的当前对象。

#### 遍历对象的属性

通过 for...in 语句用于遍历数组或者对象的属性（对数组或者对象的属性进行循环操作）。比如：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>

    <body>
        <script>
            var student = {
                name: 'zhangsan',
                age: 18,
                gender: 'male'
            };
            for(var key in student) {
                console.log(key);
                console.log(student[key]);
            }
        </script>
    </body>

</html>
```

注：key 是一个变量,这个变量中存储的是该对象的所有的属性的名字。

#### 删除对象的属性

使用 delete 删除对象的属性。比如在控制台中输入以下代码：

```javascript
var student = {
  name: 'zhangsan',
  age: 18,
  gender: 'male'
};
student.name; // "zhangsan"
delete student.name;
student.name; // undefined
```

## 四.Web API

### 4.1 API

> API（Application Programming Interface,应用程序编程接口）: " 计算机操作系统 "（Operating system）或 " 程序库 " 提供给应用程序调用使用的代码。其主要目的是让应用程序开发人员得以调用一组例程功能，而无须考虑其底层的源代码为何、或理解其内部工作机制的细节。API本身是抽象的，它仅定义了一个接口，而不涉及应用程序在实际实现过程中的具体操作。

注：定义来自维基百科。



Web API 是浏览器提供的一套操作浏览器功能和页面元素的 API(BOM 和 DOM)。

### 4.2BOM 简介

> 浏览器对象模型(BOM)指的是由 Web 浏览器暴露的所有对象组成的表示模型。BOM 与 DOM 不同，其既没有标准的实现，也没有严格的定义, 所以浏览器厂商可以自由地实现 BOM。
>
> 作为显示文档的窗口, 浏览器程序将其视为对象的分层集合。当浏览器分析文档时, 它将创建一个对象的集合, 以定义文档, 并详细说明它应如何显示。浏览器创建的对象称为文档对象。它是浏览器使用的更大的对象集合的一部分。此浏览器对象集合统称为浏览器对象模型或 BOM。
>
> BOM 层次结构的顶层是窗口对象, 它包含有关显示文档的窗口的信息。某些窗口对象本身就是描述文档和相关信息的对象。

注：定义来自维基百科。

### 4.3 BOM 的顶级对象 window 以及常用操作方法

window 是浏览器的顶级对象，当调用 window 下的属性和方法时，可以省略 window。

#### 对话框

- alert()：显示带有一段消息和一个确认按钮的警告框。
- prompt():显示可提示用户输入的对话框。
- confirm():显示带有一段消息以及确认按钮和取消按钮的对话框。

#### 页面加载事件

- onload

```javascript
window.onload = function () {
  // 当页面加载完成执行
  // 当页面完全加载所有内容（包括图像、脚本文件、CSS 文件等）执行
}
```

- onunload

```javascript
window.onunload = function () {
  // 当用户退出页面时执行
}
```

#### 浏览器尺寸

```javascript
var width = window.innerWidth
|| document.documentElement.clientWidth
|| document.body.clientWidth;

var height = window.innerHeight
|| document.documentElement.clientHeight
|| document.body.clientHeight;
```

上述代码可以获取所有浏览器的宽高(（不包括工具栏/滚动条）)。

#### 定时器

- setTimeout() 方法在指定的毫秒数到达之后执行指定的函数，只执行一次。clearTimeout() 方法取消由 setTimeout() 方法设置的 timeout。

```javascript
// 创建一个定时器，2000毫秒后执行，返回定时器的标示
var timerId = setTimeout(function () {
  console.log('Hello shiyanlou');
}, 2000);

// 取消定时器的执行
clearTimeout(timerId);
```

- setInterval() 方法设置定时调用的函数也就是可以按照给定的时间(单位毫秒)周期调用函数，clearInterval() 方法取消由setInterval() 方法设置的 timeout。

```javascript
// 创建一个定时器，每隔2秒调用一次
var timerId = setInterval(function () {
  var date = new Date();
  console.log(date.toLocaleTimeString());
}, 2000);

// 取消定时器的执行
clearInterval(timerId);
```

注：BOM 的操作方法还有很多，但是一般情况下我们常用的就是上面所介绍的。有兴趣的可以自行百度了解 BOM 的更多操作方法和介绍。

### 4.4 DOM简介

文档对象模型（Document Object Model，简称DOM），是 W3C 组织推荐的处理可扩展标志语言的标准编程接口。DOM 定义了访问 HTML 和 XML 文档的标准。我们这里主要学习 HTML DOM。DOM 可以把 HTML 看做是文档树，通过 DOM 提供的 API 可以对树上的节点进行操作。下面我们来看一下 W3C 上的 dom 树：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547429789687.png/wm)

### 4.5 DOM HTML

DOM 能够操作 HTML 的内容。

#### 改变 HTML 输出流

在 JavaScript 中，使用 document.write() 可用于直接向 HTML 输出流写内容。比如：

```javascript
document.write('新设置的内容<p>标签也可以生成</p>');
```

在控制台中复制上述代码运行后：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547437152292.png/wm)

#### 改变 HTML 内容

使用 innerHTML 属性改变 HTML 内容。比如修改 p 标签中的内容：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>

    <body>
        <p id="p1">Hello World!</p>

        <script>
            document.getElementById("p1").innerHTML = "Hello 实验楼";
        </script>

    </body>

</html>
```

#### 改变 HTML 属性

语法：

```javascript
document.getElementById(id).attribute = new value;
```

例子：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>

    <body>

        <img id="image" src="https://www.baidu.com/img/bd_logo1.png">

        <script>
            document.getElementById("image").src = "https://static.shiyanlou.com/img/shiyanlou_logo.svg";
        </script>

    </body>

</html>
```

注：上述例子将显示实验楼的 logo 图片。

### 4.6DOM CSS

DOM 能够改变 HTML 元素的样式。语法为：

```javascript
document.getElementById(id).style.property=new style;
```

例子：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>

    <body>

        <p id="syl" style="color: red;">实验楼</p>

        <script>

            document.getElementById("syl").style.color = "green";
        </script>

    </body>

</html>
```

注：在上述例子中，p 标签中实验楼的颜色本来为红色，但是通过 DOM 方法，最后将其改变成了绿色，运行上述代码，最终的效果是显示一个颜色为绿色的实验楼文本。

### 4.7 DOM节点

根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：整个文档就是一个文档节点。而每一个 HMTL 标签都是一个元素节点。HTML 标签中的文本则是文本节点。HTML 标签的属性是属性节点。一切都是节点。

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547430589989.png/wm)

#### 获取节点

要操作节点，首先我们要找到节点。主要有以下三种办法：

1. 通过 ID 找到 HTML 元素：使用方法 getElementById() 通过元素的 ID 而选取元素，比如：

```javascript
document.getElementById("demo"); // 假定已经有一个 ID 名为 demo 的标签，可以这样来获取它
```

1. 通过标签名找到 HTML 元素：使用方法 getElementsByTagName() 来选取元素，如果有多个同类型标签，那么我们可以通过下标来确认，比如：

```html
<html>
<body>
<input type="text" />
<input type="text" />

<script>
document.getElementsByTagName("input")[0].value="hello";   // 下标为 [0] 表示选取第 1 个 input 标签
document.getElementsByTagName("input")[1].value="shiyanlou"; // 下标为 [1] 表示选取第 2 个 input 标签
</script>

</body>
</html>
```

1. 通过类名来找到 HTML 元素：使用方法 getElementsByClassName() 通过元素的类名来选取元素。比如：

```javascript
document.getElementsByClassName("name"); // 返回包含 class="name" 的所有元素的一个列表。
```

#### DOM 节点直接的关系

DOM 的节点并不是孤立的，我们从 dom 树中也可以看出,节点与节点之间存在着相对的关系,就如同一个家族一样,有父辈，有兄弟，有儿子等等。下面我们来看一下都有哪些节点：

| 父节点     | 兄弟节点               | 子节点            | 所有子节点 |
| ---------- | ---------------------- | ----------------- | ---------- |
| parentNode | nextSibling            | firstChild        | childNodes |
|            | nextElementSibling     | firstElementChild | children   |
|            | previousSibling        | lastChild         |            |
|            | previousElementSibling | lastElementChild  |            |

例子：

```html
<html>
  <head>
    <title>DOM 节点演示</title>
  </head>
  <body>
    <h1>我是h1标签</h1>
    <p>我是p标签</p>
  </body>
</html>
```

上面的例子中：

- <html>节点没有父节点,它是根节点。
- <head> 和 <body> 的父节点是 <html> 节点
- 文本节点 "我是p标签"的父节点是 <p> 节点
- <html>节点有两个子节点: <head> 和 <body>
- <h1>节点和<p>节点是兄弟节点,同时也是<body>的子节点

需要注意以下几点：

- childNodes：它是标准属性，它返回指定元素的子元素集合，包括 HTML 节点，所有属性，文本节点
- children：非标准属性，它返回指定元素的子元素集合。但它只返回 HTML 节点，甚至不返回文本节点。
- nextSibling 和 previousSibling 获取的是节点，获取元素对应的属性是 nextElementSibling 和 previousElementSibling。
- nextElementSibling 和 previousElementSibling 有兼容性问题，IE9 以后才支持。

#### DOM 节点的操作

1. 创建节点

   - 创建元素节点：使用 createElement() 方法。比如：

     ```javascript
     var par = document.createElement("p");
     ```

   - 创建属性节点：使用 createAttribute() 方法。

   - 创建文本节点：使用 createTextNode() 方法。

2. 插入子节点

   - appendChild () 方法向节点添加最后一个子节点。
   - insertBefore (插入的新的子节点，指定的子节点) 方法在指定的子节点前面插入新的子节点。如果第二个参数没写或者为 null，则默认插入到后面。

3. 删除节点:使用 removeChild() 方法。写法为：

```javascript
父节点.removeChild(子节点);
node.parentNode.removeChild(node); //如果不知道父节点是什么，可以这样写
```

1. 替换子节点：使用 replaceChild() 方法。语法为：

```javascript
node.replaceChild(newnode,oldnode);
```

1. 设置节点的属性：
   - 获取：getAttribute(名称)
   - 设置：setAttribute(名称, 值)
   - 删除：removeAttribute(名称)

### 4.8DOM 事件

#### 事件的定义

在什么时候执行什么事。

#### 事件三要素

事件由：事件源 + 事件类型 + 事件处理程序组成。

- 事件源：触发事件的元素。
- 事件类型：事件的触发方式(比如鼠标点击或键盘点击)。
- 事件处理程序：事件触发后要执行的代码(函数形式，匿名函数)。

#### 常用的事件

| 事件名      | 说明                                 |
| ----------- | ------------------------------------ |
| onclick     | 鼠标单击                             |
| ondblclick  | 鼠标双击                             |
| onkeyup     | 按下并释放键盘上的一个键时触发       |
| onchange    | 文本内容或下拉菜单中的选项发生改变   |
| onfocus     | 获得焦点，表示文本框等获得鼠标光标。 |
| onblur      | 失去焦点，表示文本框等失去鼠标光标。 |
| onmouseover | 鼠标悬停，即鼠标停留在图片等的上方   |
| onmouseout  | 鼠标移出，即离开图片等所在的区域     |
| onload      | 网页文档加载事件                     |
| onunload    | 关闭网页时                           |
| onsubmit    | 表单提交事件                         |
| onreset     | 重置表单时                           |

例子 1 ：鼠标单击事件：

```html
<p onclick="this.innerHTML='我爱学习，身体好好!'">请点击该文本</p>
```

例子2 ：鼠标双击事件：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>

    </head>

    <body>

        <h1 ondblclick="changetext(this)">请点击该文本</h1>
        <script>
            function changetext(id) {
                id.innerHTML = "我爱学习，身体棒棒!"
            }
        </script>

    </body>

</html>
```

例子3 ：鼠标移除悬停：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>

    </head>

    <body>

        <div onmouseover="mOver(this)" onmouseout="mOut(this)" style="background-color:deepskyblue;width:200px;height:100px;">把鼠标移到上面</div>
        <script>
            function mOver(obj) {
                obj.innerHTML = "你把鼠标移到了上面 ";
            }

            function mOut(obj) {
                obj.innerHTML = "你把鼠标移开了";
            }
        </script>

    </body>

</html>
```

注：大家可以把上述例子运行一下感受一下事件的魅力。并且可以自己尝试着写一些其他的简单事件。

## 实例：导航栏切换

通过鼠标点击更改导航栏的样式，来看看最终的效果：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547453308407.png/wm)

参考源码：

```html
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            #list li {
                list-style-type: none;
                width: 100px;
                height: 50px;
                line-height: 50px;
                background-color: beige;
                text-align: center;
                float: left;
            }

            #list li.current {
                background-color: red;
            }

            #list li a {
                text-decoration: none;
            }
        </style>

    </head>

    <body>
        <div id="menu">
            <ul id="list">
                <li class="current">
                    <a href="javascript:void(0)">首页</a>
                </li>
                <li>
                    <a href="javascript:void(0)">HTML</a>
                </li>
                <li>
                    <a href="javascript:void(0)">CSS</a>
                </li>
                <li>
                    <a href="javascript:void(0)">JavaScript</a>
                </li>
                <li>
                    <a href="javascript:void(0)">关于</a>
                </li>
                <li>
                    <a href="javascript:void(0)">帮助</a>
                </li>
            </ul>
        </div>

        <script>
            //获取所有的li标签,
            var liObjs = document.getElementById("list").getElementsByTagName("li");
            //循环遍历,找到每个li中的a,注册点击事件
            for(var i = 0; i < liObjs.length; i++) {
                //每个li中的a
                var aObj = liObjs[i].firstElementChild;

                aObj.onclick = function() {
                    //把这个a所在的li的所有的兄弟元素的类样式全部移除
                    for(var j = 0; j < liObjs.length; j++) {
                        liObjs[j].removeAttribute("class");
                    }
                    //当前点击的a的父级元素li(点击的这个a所在的父级元素li),设置背景颜色
                    this.parentNode.className = "current";
                };
            }
        </script>

    </body>

</html>
```

## 五.值类型和引用类型以及异常

### 5.1值类型和引用类型

#### 值类型

值类型又叫基本数据类型，在 JavaScript 中值类型有以下五种：

- 数值类型
- 布尔类型
- undefined
- null
- 字符串

值类型存储在栈（stack）中，它们的值直接存储在变量访问的位置。比如：

```js
var num = 18;
var flag = true;
var un = undefined;
var nu = null;
var str = "zhangsan";
```

上面定义的这些值类型的数据在内存中的存储如下图所示：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547607159775.png/wm)

#### 引用类型

引用类型又叫复合数据类型，在 JavaScript 中引用类型有以下三种：

- 对象
- 数组
- 函数

引用类型存储在堆中，也就是说存储在变量处的值是一个指针，指向存储对象的内存处。比如：

```js
var arr = [1, 2, 3];
var p = {name:"张三", age:18};
```

上面定义的这些引用类型的数据在内存中的存储如下图所示：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547609082688.png/wm)

### 5.2值类型和引用类型的特征

#### 值类型的特征

- 值类型的值是不可变的，不可变是指值类型指向的空间不可变。比如：

```js
var a = 2;
a = a + 2;
console.log(a); //打印结果为 4。
```

在上述例子中，a 变量指向的值变了，但是 2 的内存没有变。

- 按值传递的变量之间互不影响。比如：

```js
var a = 1;
var b = a;
a = a + 2;
console.log(a,b); // 打印结果为3,1
```

- 值类型赋值，直接将值赋值一份。比如：

```js
var num1 = 10;
var num2 = num1;
```

上述代码在内存中的体现为：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547617637137.png/wm)

- 当参数为值类型的时候，函数内和函数外的两个变量完全不同，仅仅只是存的值一样而已，修改时互不影响。比如：

```js
function foo(num){
    num = num + 1;
}

var a = 1;
foo(a);
console.log(a); //打印结果为 1
```

#### 引用类型的特征

- 引用类型赋值，是将地址复制一份。比如：

```js
var p = {name:"zhangsan", age:18};
var p1 = p;
```

上述代码在内存中的体现为：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547618081944.png/wm)

再来看一段代码：

```js
var p = {name:"张三", age:18};
var p1 = p; 
console.log(p.name); // 打印结果为张三
console.log(p1.name); // 打印结果为张三
p.name = "李四";
console.log(p.name); // 打印结果为李四
console.log(p1.name); // 打印结果为李四
```

- 当参数为引用类型的时候，函数内和函数外的两个变量不同，但是共同指向同一个对象，在函数内修改对象数据时会影响外部。比如：

```js
function foo(o){
    o.age = o.age +1;
}

var p = {name:"zhangsan", age:18};

foo(p);

console.log(p.age); // 打印结果为 19。
```

注：其实我们可以这样理解。引用类型中的地址是一把钥匙，钥匙指向的是宝藏，复制一把钥匙后，两把钥匙能打开的是同一个宝藏。

### 5.3 调试工具的使用

在编写 JavaScript 代码时，如果遇见错误，首先在浏览器中运行我们的代码，然后 F12，查看错误信息。比如运行以下代码：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <script>
            alerttt("hello");
        </script>
    </body>
</html>
```

首先查看控制台信息，在控制台中会有报错提示，一般看这个就能知道错误问题是什么了。

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547623603079.png/wm)

在 Sources 中能够清楚的看到哪一行的代码出问题了，会有很显眼的提醒。

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547623721469.png/wm)

如果我们想知道变量的值，在调试的时候，可以加一句 console.log(变量) 语句来打印出来，然后在控制台中看。console.log() 语句也是我们编程中经常需要使用的，因为有时候，我们也不能直观的一下就知道传递进来的值到底是什么，可能需要看半天的逻辑然后计算半天，直接打印出来的方式也有利于帮助我们判断，不过一般情况下大家记得调试完，要把这行语句注释掉或者删掉。

#### 设置断点，逐步执行

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <script>
            for(var i=1;i<=100;i++)
            {
                for (var j=1;j<=10;i++) {
                    j=i*j;
                    console.log(j);
                }
            }
        </script>
    </body>
</html>
```

首先运行上述代码，点击 F12 进行调试界面，点击 Sources ，设置断点的方法很简单，直接左键点击代码旁边的数字行数。如下所示：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547625264168.png/wm)

将变量添加到 watch 窗口，实时查看它的值的变化，如下所示：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547625569932.png/wm)

准备工作做好之后我们还需要点击一下刷新，如下所示，红色箭头所指的地方点击一下：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547625739780.png/wm)

然后就可以通过点击运行按钮，逐行运行我们设置断点时的代码，并在 watch 中查看变量值的变化，如下所示：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547625872548.png/wm)

### 5.4异常处理

在我们实际编码过程中，经常会遇到各种各样的错误，有可能是语法错误，也可能是拼写错误，也可能是浏览器的兼容性问题或者就是些莫名其妙的原因。当出现错误时，JavaScript 引擎通常会停止，并生成一个错误信息。那我们应该怎么来调试我们的代码呢？

#### 异常捕获

我们使用 try-catch 语句开捕获异常，语法为：

```javascript
try{
    //这里写可能出现异常的代码
}catch(err){
    //在这里写，出现异常后的处理代码
}
```

例子：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>

    <body>
        <input type="button" value="点击一下" onclick="message()">
        <script>
            var txt = "";

            function message() {
                try {
                    alertt("我爱学习，身体好好"); //故意把alert写错
                } catch(err) {
                    txt = "There was an error on this page.\n\n";
                    txt += "Error description: " + err.message + "\n\n";
                    txt += "Click OK to continue.\n\n";
                    alert(txt);
                }
            }
        </script>
    </body>

</html>
```

需要注意以下几点：

- 语句 try 和 catch 是成对出现的。
- 如果在 try 中出现了错误, try 里面出现错误的语句后面的代码都不再执行, 直接跳转到 catch 中，catch 处理错误信息，然后再执行后面的代码。
- 如果 try 中没有出现错误，则不会执行 catch 中的代码，直接执行后面的代码。通过 try-catch 语句进行异常捕获之后，代码将会继续执行，而不会中断。

#### throw 语句

通过 throw 语句，我们可以创建自定义错误。throw 语句常常和 try catch 语句一起使用。

例子：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>

    <body>
        <p>请输入 0 到 100 之间的数字：</p>
        <input id="demo" type="text">
        <button type="button" onclick="myFunction()">测试输入值</button>
        <p id="throwText"></p>
        <script>
            function myFunction() {
                try {
                    var x = document.getElementById("demo").value;
                    if(x == "") throw "您输入的值为空";
                    if(isNaN(x)) throw "您输入的不是数字";
                    if(x > 100) throw "您输入的值太大";
                    if(x < 0) throw "您输入的值太小";
                } catch(err) {
                    var y = document.getElementById("throwText");
                    y.innerHTML = "错误提示：" + err + "。";
                }
            }
        </script>
    </body>

</html>
```

## 六.面向对象编程

### 6.1面向对象编程实例

下面我们通过一个例子，来感受一下什么叫面向对象编程。比如我们设置页面中的 div 标签 和 p 标签的背景色为 color。如果按照我们前面所需我们可能会这样写：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            div,p{
                width: 200px;
                height: 100px;
            }
        </style>
    </head>
    <body>
        <div>你好吗？</div>
        <p>我很好</p>
        <div>测试一下嘛</div>
        <p>好的啊</p>
        <script>
            var divs = document.getElementsByTagName("div");
            for(var i = 0;i < divs.length;i++){
                divs[i].style.backgroundColor = "red";
            }
            var ps = document.getElementsByTagName("p");
            for(var j = 0;j < ps.length;j++){
                ps[j].style.backgroundColor = "red";
            }
        </script>
    </body>
</html>
```

是不是觉得有点麻烦？好像有重复的？有的人可能会想到用函数来封装一下相同的代码:

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            div,p{
                width: 200px;
                height: 100px;
            }
        </style>
    </head>
    <body>
        <div>你好吗？</div>
        <p>我很好</p>
        <div>测试一下嘛</div>
        <p>好的啊</p>
        <script>
            function getTagname(tagName){
                return document.getElementsByTagName(tagName);
            }
            function setStyle(arr){
                for(var i = 0;i < arr.length;i++){
                    arr[i].style.backgroundColor = "red";
                }
            }
            var divs = getTagname("div");
            setStyle(divs);
            var ps = getTagname("p");
            setStyle(ps);
        </script>
    </body>
</html>
```

我们再来看看使用面向对象的方式：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            div,
            p {
                width: 200px;
                height: 100px;
            }
        </style>
    </head>

    <body>
        <div>你好吗？</div>
        <p>我很好</p>
        <div>测试一下嘛</div>
        <p>好的啊</p>
        <script>
            var test = {
                getEle: {
                    //实际上本案例只需要写 tag，但是为了体现面向对象的思想，我们把获取获取节点的三种方式都写出来
                    tag: function(tagName) {
                        return document.getElementsByTagName(tagName);
                    },
                    id: function(idName) {
                        return document.getElementById(idName);
                    },
                    class: function(className) {
                        return document.getElementsByClassName(className);
                    }
                },
                //实际上本案例只需要写 setStyle，同样的为了体现面向对象编程的思想，我们可以设置添加移除修改样式的函数。
                setCss: {
                    setStyle: function(arr) {
                        for(var i = 0; i < arr.length; i++) {
                            arr[i].style.backgroundColor = "red";
                        }
                    },

                    updateCss: function() {},
                    deleteCss: function() {}
                    // ... 
                }

            };

            var divs = test.getEle.tag("div");
            test.setCss.setStyle(divs);
            var ps = test.getEle.tag("p");
            test.setCss.setStyle(ps);
        </script>
    </body>

</html>
```

### 6.2构造函数

首先，我们来复习一下创建对象的方式。

1. 通过对象字面量来创建。

```js
var student = {
  name: 'zhangsan',
  age: 18,
  gender : 'male',
  sayHi: function () {
    console.log("hi,my name is "+this.name);
  }
}; 
```

1. 通过 new Object() 创建对象。

```js
var student = new Object();
  student.name = 'zhangsan',
  student.age = 18,
  student.gender = 'male',
  student.sayHi = function () {
    console.log("hi,my name is "+this.name);
  }
```

上面两种都是简单的创建对象的方式，但是如果有两个 student 实例对象呢？cv（Ctrl C + Ctrl V） 大法？分别命名一下 student1？student2？那如果一个班的学生？n 个学生？显然如果这样做的法代码冗余率太高，是不可取的。我们也学过函数，所以简单方式的改进是：工厂函数。

1. 通过工厂函数来创建对象。

```js
function createStudent(name, age, gender) {
  var student = new Object();
  student.name = name;
  student.age = age;
  student.gender = gender;
  student.sayHi = function(){
    console.log("hi,my name is "+this.name);
  }
  return student;
}
var s1 = createStudent('zhangsan', 18, 'male');
var s2 = createStudent('lisi', 19, 'male');
```

这样封装代码确实解决了代码冗余的问题，但是每次调用函数 createStudent() 都会创建新函数 sayHi(),也就是说每个对象都有自己的 sayHi() 版本，而事实上，每个对象都共享一个函数。为了解决这个问题，我们引入面向对象编程里的一个重要概念：构造函数。

1. 通过构造函数来创建对象。

```js
function Student(name,age,gender){
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.sayHi = function(){
       console.log("hi,my name is "+this.name);
  }
}
var s1 = new Student('zhangsan', 18, 'male');
```

来看看构造函数与工厂函数的区别：

- 首先在构造函数内没有创建对象，而是使用 this 关键字，将属性和方法赋给了 this 对象。
- 构造函数内没有 return 语句，this 属性默认下是构造函数的返回值。
- 函数名使用的是大写的 Student。
- 用 new 运算符和类名 Student 创建对象。

**构造函数存在的问题**

构造函数虽然科学，但仍然存在一些问题

我们使用前面的构造函数例子来讲解(修改了 sayHi 方法)：

```js
function Student(name,age,gender){
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.sayHi = function(){
       console.log("hi");
  }
}
```

首先我们创建两个实例化对象：

```js
var s1 = new Student('zhangsan', 18, 'male');
s1.sayHi(); //打印 hi
var s2 = new Student('lisi',18,'male');
s2.sayHi(); //打印 hi
console.log(s1.sayHi == s2.sayhi); //结果为false
```

眼见为实，来看看效果：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547694897490.png/wm)

由于每个对象都是由 new Student 创建出来的，因此每创建一个对象，函数 sayHi 都会被重新创建一次，这个时候，每个对象都拥有一个独立的，但是功能完全相同的方法，这样势必会造成内存浪费。有的人可能会想，既然是一样的那我们就单独把它提出来，写一个函数，每次调用不就可以了吗？比如：

```js
function sayHi(){
    console.log("hi");
}
function Student(name,age,gender){
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.sayHi = sayHi;
}
var s1 = new Student('zhangsan', 18, 'male');
s1.sayHi(); //打印 hi
var s2 = new Student('lisi',18,'male');
s2.sayHi(); //打印 hi
console.log(s1.sayHi == s2.sayHi);//结果为true
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547702361417.png/wm)

但是这样做会导致全局变量增多，可能会引起命名冲突，代码结果混乱，维护困难。通过使用原型可以很好的解决这个问题。

### 6.3 原型

原型：prototype

在 JavaScript 中，每一个函数都有一个 prototype 属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数的实例继承。我们来看看前面例子原型的写法：

```js
function Student(name, age, gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
}
Student.prototype.sayHi = function(){
    console.log("hi");
}
var s1 = new Student('zhangsan', 18, 'male');
s1.sayHi();//打印 hi
var s2 = new Student('lisi', 18, 'male');
s2.sayHi();//打印 hi
console.log(s1.sayHi == s2.sayHi);//结果为true
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547702473320.png/wm)

**构造函数、实例、原型三者之间的关系**

我们之前提到过：每一个函数都有一个 prototype 属性，指向另一个对象。让我们用代码验证一下，在编辑器中输入以下代码：

```js
<script type="text/javascript">
    function F() {}
    console.log(F.prototype);
</script>
```

上述代码在浏览器中打印结果为 Object，验证了我们所说的 prototype 属性，指向另一个对象。

构造函数的 prototype 对象默认都有一个 constructor 属性，指向 prototype 对象所在函数。在控制台中运行下面的代码：

```js
function F() {}
console.log(F.prototype.constructor === F);//结果为ture
```

通过构造函数得到的实例对象内部会包含一个指向构造函数的 prototype 对象的指针__proto__。__proto__属性最早是火狐浏览器引入的，用以通过实例对象来访问原型，这个属性在早期是非标准的属性。在控制台中运行下面的代码：

```js
function F() {}
var a = new F();
console.log(a.__proto__ === F.prototype); //结果为true
```

实例对象可以直接访问原型对象成员。所有实例都直接或间接继承了原型对象的成员。

总结：每个构造函数都有一个原型对象，原型对象包含一个指向构造函数的指针 constructor，而实例都包含一个指向原型对象的内部指针__proto__ 。

### 6.4原型链

我们说过所有的对象都有原型，而原型也是对象，也就是说原型也有原型，那么如此下去，也就组成了我们的原型链。

#### 属性搜索原则

属性搜索原则，也就是属性的查找顺序，在访问对象的成员的时候，会遵循以下原则：

1. 首先从对象实例本身开始找，如果找到了这个属性或者方法，则返回。
2. 如果对象实例本身没有找到，就从它的原型中去找，如果找到了，则返回。
3. 如果对象实例的原型中也没找到，则从它的原型的原型中去找，如果找到了，则返回。
4. 一直按着原型链查找下去，找到就返回，如果在原型链的末端还没有找到的话，那么如果查找的是属性则返回 undefined，如果查找的是方法则返回 xxx is not a function。

#### 更简单的原型语法

在前面的例子中，我们是使用 xxx.prototype. 然后加上属性名或者方法名来写原型，但是每添加一个属性或者方法就写一次显得有点麻烦，因此我们可以用一个包含所有属性和方法的对象字面量来重写整个原型对象：

```js
function Student(name, age, gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
}
Student.prototype = {
    hobby:"study",
    sayHi:function(){
    console.log("hi");
    }
}
var s1 = new Student("wangwu",18,"male");
console.log(Student.prototype.constructor === Student);//结果为 false
```

但是这样写也有一个问题，那就是原型对象丢失了 constructor 成员。所以为了保持 constructor 成员的指向正确，建议的写法是：

```js
function Student(name, age, gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
}
Student.prototype = {
    constructor: Student, //手动将 constructor 指向正确的构造函数
    hobby:"study",
    sayHi:function(){
    console.log("hi");
    }
}
var s1 = new Student("wangwu",18,"male");
console.log(Student.prototype.constructor === Student);//结果为 true
```

### 6.5原型链继承

我们都听过这么一句话：子承父业。而在我们的 JavaScript 中也有继承。接下来我们会学习原型链继承。原型链继承的主要思想是利用原型让一个引用类型继承另外一个引用类型的属性和方法。

```js
function Student(name, age, gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
}
Student.prototype.sayHi = function(){
    console.log("hi");
}
var s1 = new Student("zhangsan",18,"male");
s1.sayHi(); //打印 hi
var s2 = new Student("lisi",18,"male");
s2.sayHi(); //打印 hi
```

上述例子中实例化对象 s1 和 s2 都继承了 sayHi 方法。

**Object.prototype 成员介绍**

在控制台中输入以下代码：

```js
Object.prototype;
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547709166287.png/wm)

我们介绍常用的几个 Object.prototype 成员：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547709733167.png/wm)

## 七.函数进阶

### 7.1call、apply、bind

在学习 call、apply、bind 方法之前，我们先来复习一下 this 的指向问题，我们前面说过一个口诀：谁调用 this，它就指向谁。让我们先来来看一个例子：

```js
function foods() {
}
foods.prototype = {
    price: "￥15",
    say: function() {
        console.log("My price is " + this.price);
    }
}

var apple = new foods();
apple.say();    //My price is ￥15
var orange = new foods();
orange.say();  // My price is ￥15
```

也就是说上述例子调用 say 方法，最后打印的结果都是一样的，但是如果我们想打印橘子的价钱是 10 元呢？又不想重新定义 say 方法。JavaScript 为我们专门提供了一些函数方法用来帮我们更优雅的处理函数内部 this 指向问题。这就是接下来我们要学习的 call、apply、bind 三个函数方法。

#### call

call() 方法调用一个函数, 其具有一个指定的 this 值和分别地提供的参数(参数的列表)。语法为：

```js
fun.call(thisArg, arg1, arg2, ...)
```

注：

- thisArg 指的是在 fun 函数中指定的 this 的值。如果指定了 null 或者 undefined 则内部 this 指向 window，同时值为原始值(数字，字符串，布尔值)的 this 会指向该原始值的自动包装对象。是一个可选项。
- arg1, arg2, ...指定的参数列表。也是可选项。
- 使用调用者提供的 this 值和参数调用该函数的返回值。若该方法没有返回值，则返回 undefined。
- call() 允许为不同的对象分配和调用属于一个对象的函数/方法。
- call() 提供新的 this 值给当前调用的函数/方法。你可以使用 call 来实现继承：写一个方法，然后让另外一个新的对象来继承它（而不是在新对象中再写一次这个方法）。

1. 使用 call() 方法调用函数并且指定上下文的 'this'。前面的例子可以改写成：

   ```js
    function foods() {
    }
    foods.prototype = {
        price: "￥15",
        say: function() {
            console.log("My price is " + this.price);
        }
    }
    var apple = new foods();
    orange = {
        price: "￥10"
    }
    apple.say.call(orange); // My price is ￥10
   ```

2. 在一个子构造函数中，你可以通过调用父构造函数的 call() 方法来实现继承。在控制台输入如下代码：

   ```js
    function Father(name, age) {
      this.name = name;
      this.age = age;
    }
   
    function Son(name, age) {
      Father.call(this, name, age);
      this.hobby = 'study';
    }
   
    var S1 = new Son('zhangsan', 18);
    S1; // Son {name: "zhangsan", age: 18, hobby: "study"}
   ```

#### apply

apply() 方法与 call() 方法类似，唯一的区别是 call() 方法接受的是参数，apply() 方法接受的是数组。语法为：

```js
fun.apply(thisArg, [argsArray])
```

1. 使用 apply() 方法将数组添加到另一个数组。

   例子：

```js
    var array = ['a', 'b','c'];
    var nums = [1, 2, 3];
    array.push.apply(array, nums);
    array //["a", "b", "c", 1, 2, 3]
注：concat() 方法连接数组，不会改变原数组，而是创建一个新数组。而使用 push 是接受可变数量的参数的方式来添加元素。使用 apply 则可以连接两个数组。
```

1. 使用 apply() 方法和内置函数。

   例子：

```js
        var numbers = [7, 10, 2, 1, 11, 9];
        var max = Math.max.apply(null, numbers); 
        max; //11
 注：直接使用 max() 方法的写法为： Math.max(7, 10, 2, 1, 11, 9);
```

#### bind

bind() 方法创建一个新的函数（称为绑定函数），在调用时设置 this 关键字为提供的值。并在调用新函数时，将给定参数列表作为原函数的参数序列的前若干项。语法为：

```js
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

注：参数 thisArg ：当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用 new 操作符调用绑定函数时，该参数无效。参数：arg1, arg2, ...表示当目标函数被调用时，预先添加到绑定函数的参数列表中的参数。

我们创建一个简单的绑定函数例子：

```js
var bin = function(){
    console.log(this.x);
}
var foo = {
x:10
}
bin(); // undefined
var func = bin.bind(foo); //创建一个新函数把 'this' 绑定到 foo 对象
func(); // 10
```

我们再来看一个例子：

```js
this.num = 6;
var test = {
  num: 66,
  getNum: function() { return this.num; }
};

test.getNum(); // 返回 66

var newTest = test.getNum;
newTest(); // 返回 6, 在这种情况下，"this"指向全局作用域

// 创建一个新函数，将"this"绑定到 test 对象
var bindgetNum = newTest.bind(test);
bindgetNum(); // 返回 66
var newTest = test.getNum;
newTest(); 

//上面这两行代码其实相当于：

var newTest(){
    return this.num;
}
//所以 this 指向的是全局作用域，返回 6。
```

### 7.2 递归

在程序中，递归就是函数自己直接或者间接的调用自己。

例子:计算1到10之间的整数相加的和：

```js
function foo(n){
    if ( n == 0 ) {
        return 0;
    }//临界条件
    else{
        return n + foo( n - 1 );
    }  
}
var a = foo(10);
a; // 55
```

注：一定要写临界条件，不然程序无法结束并且会报错。

### 7.3 作用域

作用域值的就是作用范围，也就是说一个变量在什么地方可以使用，在什么地方不能使用。

#### 块级作用域

在 JavaScript 中是没有块级作用域的。比如：

```js
{
    var num = 123;
    {
        console.log( num ); 
    }
}
console.log( num );
```

上面的例子并不会报错，而是打印两次 123，但是在其他编程语言中（C#、C、JAVA）会报错，这是因为在 JavaScript 中是没有块级作用域的也就是说，使用 {} 标记出来的代码块中声明的变量 num，是可以被 {} 外面访问到的。

#### 函数作用域

JavaScript 的函数作用域是指在函数内声明的所有变量在函数体内始终是可见的，不涉及赋值。来看个例子：

```js
function test() {
    var num = 123;
    console.log(num);
    if(2 == 3) {
        var k = 5;
        for(var i = 0; i < 10; i++) {

        }
        console.log(i);
    }
    console.log(k); //不会报错，而是显示undefined
}
test();
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547884049674.png/wm)

我们把上面的代码 if 语句中的条件改为 2 == 2 来看看效果：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547884177854.png/wm)

我们再来对比一下如果没有在函数体内会是什么效果：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547884380455.png/wm)

#### 全局作用域

全局作用域也就是说什么地方都能够访问到。比如我们不用 var 关键字，直接声明变量的话，那个变量就是全局变量，它的作用域就是全局作用域。使用 windows 全局对象来声明，全局对象的属性也是全局变量。另外在所有的函数外部用 var 声明的变量也是全局变量，这是因为内层作用域可以访问外层作用域。

注：

- 内层作用域可以访问外层作用域，反之不行。
- 整个代码结构中只有函数可以限定作用域。
- 如果当前作用规则中有名字了, 就不考虑外面的同名变量。
- 作用域规则首先使用提升规则分析。

#### 变量名提升

JavaScript 是解释型的语言，但是它并不是真的在运行的时候完完全全的逐句的往下解析执行。我们来看个例子：

```js
func();

function func(){
     console.log("Hello syl");
}
```

在控制台中运行效果为：

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547797629301.png/wm)

这说明了它并不是完全的逐句往下解析的，否则是会报错的。显然，在执行func()之前，引擎就已经解析到了function func(){},发生了变量名提升。那么变量名提升是在什么时候发生的呢？JavaScript 引擎在对 JavaScript 代码进行解释执行之前，会对 JavaScript 代码进行预解析，在预解析阶段，会将以关键字 var 和 function 开头的语句块提前进行处理：当变量和函数的声明处在作用域比较靠后的位置的时候，变量和函数的声明会被提升到作用域的开头。也就是说上面的代码，我们可以理解为：

```js
function func(){
     console.log("Hello syl");
}
func();
```

再来看看变量声明的例子：

```js
console.log(num);
var num = 10;
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547798103346.png/wm)

结果可能有些出人意料，但是我们这里说的提示，是声明的提升，也就是说上面的代码，我们可以理解为：

```js
var num; //这里是声明
console.log(num);//变量声明之后并未有初始化和赋值操作，所以这里是 undefined
num = 10;
```

下面再来看几个复杂一点的例子。

函数同名的时候：

```js
func();
function func(){
     console.log("Hello syl");
}

func();
function func(){
     console.log("hi syl");
}
```

上面代码相当于：

```js
function func(){
     console.log("Hello syl");
}
function func(){
     console.log("hi syl");
}
func();
func();
```

函数变量同名的时候：

```js
console.log(foo);
function foo(){}
var foo = 6;
```

当出现变量声明和函数同名的时候，只会对函数声明进行提升，变量会被忽略。所以上面的代码相当于：

```js
function foo(){};
console.log(foo);
foo = 6;
```

我们再来看一种：

```js
var num = 1;
function num () {
     alert( num );
}
num();
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547800169327.png/wm)

上面的代码相当于：

```js
function num(){
     alert(num);
}

num = 1;
num();
```

下面我们来看一个思考题：

```js
var num = 3;
function foo(){
     console.log(num); 
     var num = 4;
     console.log(num);
}
foo();
```

注：大家可以先思考一下，然后根据我们前面所学的变量名提升，试着写一下等价的代码，最后再复制上述代码到控制台中运行看看结果。

上面的代码相当于：

```js
var num = 3;
function foo(){
     var num; // 在函数顶部声明了局部变量，覆盖了函数体外同名的全局变量
     console.log(num);  // 变量存在，但是它的值为 undefined
     num = 4; // 将其初始化赋值。
     console.log(num); //打印我们期望的值 4
}
```

### 7.4 闭包

闭包是指函数可以使用函数之外定义的变量。

#### 简单的闭包

在 JavaScript 中，使用全局变量是一个简单的闭包实例。比如：

```js
var num = 3;
function foo(){
    console.log(num);
}
foo(); //打印 3 
```

#### 复杂的闭包

```js
function f1(){
    var num1 = 6;
    function f2(){
        var num2 = 7;
    }
    console.log(num1 + num2);
}
f1()
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547804618008.png/wm)

在上述代码中函数 f2 能够访问到它外层的变量 num，但是 f1 是不能访问 f2 中的变量的，因此我们可以把 num2 作为 f2 的返回值，再把 f2 作为返回值就可以访问到了。

```js
function f1(){
    var num1 = 6;
    function f2(){
        var num2 = 7;
        return num2;
    }
    return f2();
    console.log(num1 + num2);
}
f1()
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547804763043.png/wm)

### 7.5 argumnets对象

在函数代码中，使用特殊对象 arguments，无需明确指出参数名，我们就能访问它们。第一个参数是 arguments[0]，第二个参数是 arguments[1]，以此类推。比如：

```js
function foo() {
    console.log(arguments[0]);
    console.log(arguments[1]);

}
foo(2,3);//打印 2 3
```

还可以用 arguments 对象检测函数的参数个数，引用属性 arguments.length 即可。来看一个遍历参数求和的例子：

```js
function add() {
    var sum =0;
    for(var i=0; i<arguments.length; i++){
        sum += arguments[i];
    }
    return sum;
}
add();  // 0
add(1);  // 1
add(1,2); // 3  
add(1,2,3); // 6
```

### 7.6 Function对象

用 Function 对象创建函数的语法如下：

```js
var function_name = new Function(arg1, arg2, ..., argN, function_body)
```

注：每个参数都必须是字符串，function_body 是函数主体，也就是要执行的代码。

例子：

```js
var add = new Function("a","b","console.log(a+b);");
add(2,5); //打印7
```

再看一个例子:

```js
var add = new Function("a","b","console.log(a+b);");
var doAdd = add;
doAdd(2,5); // 打印7
add(2,5); // 打印7
```

在上述例子中，变量 add 被定义为函数，然后 doAdd 被声明为指向同一个函数的指针。用这两个变量都可以执行该函数的代码，并输出相同的结果。因此，函数名只是指向函数的变量，那么我们可以把函数作为参数传递给另一个函数，比如下面的例子：

```js
function addF(foo,b,c){
    foo(b,c);
}
var add = new Function("a","b","console.log(a+b);");
addF(add,2,5); // 打印7
```

#### Function 对象的 length 属性

函数属于引用类型，所以它们也有属性和方法。length 属性声明了函数期望的参数个数。

例子：

```js
var add = new Function("a","b","console.log(a+b);");
console.log(add.length); // 打印2
```

#### Function 对象的方法

Function 对象也有与所有对象共享的 valueOf() 方法和 toString() 方法。这两个方法返回的都是函数的源代码。

例子：

```js
var add = new Function("a","b","console.log(a+b);");
add.valueOf();
add.toString();
```

![img](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547866078578.png/wm)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid897174labid9222timestamp1547866243634.png/wm)

