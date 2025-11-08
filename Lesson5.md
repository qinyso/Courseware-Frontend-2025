# 蓝山工作室前端组

# Lesson5--Javascript核心(ES6)

<img src="https://cdn.jsdelivr.net/gh/qinyso/pic-bed/%E5%9B%BE%E7%89%871.png"></img>

## 课程之前

> 学习本节课之前我希望你对以下内容有基本的了解
> 
> 1. Javascript6个数据类型,变量声明方式
> 
> 2. 数组的常用方法
> 
> 3. js选择循环结构、函数相关知识
>    <a src="https://lanshanteam.feishu.cn/wiki/PVsrwWYXei5PgDkkdzWcTSh9nFg?from=from_copylink">上节课的课件</a>

------

## 那么什么是ES6呢?

> 在上节课我们知道了js核心主要包含三部分:**ECMAScript**、**DOM**、**BOM**
> 
> 而ES6是ECMAScript的第6个版本,也叫做**ECMAScript 2015**，是JavaScript语言的下一代标准。简单说：ES6 = 2015 年更新后的、更好用的 JavaScript。
> 
> 1. 把 ES 想象成一本**游戏规则手册**，里面写死了 “NPC应该设置人设,任务,外貌”—— 它只负责 “写死规则”，不负责 “做出游戏”。**(^∇^) **
> 2. 而 JS 就是 “**根据这本手册做出的具体游戏**”：
> 3. 比如浏览器里的 JS、Node.js 环境的 JS，它们就像不同厂家按《游戏规则手册》开发的同款游戏 —— 都严格遵守手册里的规定（比如必须有变量、函数这些 “角色”，必须支持加减乘除这些 “动作”），但具体怎么让这些规则跑起来（比如代码怎么解析、运算速度快不快），是每个 “游戏” 自己实现的

------

## 模板字符串

- > [!NOTE]
  > 模板字符串是ES6中引入的一种新的字符串语法。它允许在字符串中插入变量或表达式，而不需要使用字符串拼接符号。模板字符串使用反引号``包围，并使用`${}`语法来插入变量或表达式。
  
  1. ##### **变量拼接**
  
  ```javascript
  const name="小张";
  const age= 19;
  const str =`他是${name},今年${age}岁`;
  console.log(str);//输出他是小张,今年19岁
  ```
  
  2. ##### **简单表达式计算**
  
  ```javascript
  const a = 5;
  const b = 3;
  
  // 直接嵌入加减乘除
  const result = `5 + 3 = ${a + b}，5 * 3 = ${a * b}`;
  console.log(result); // 输出：5 + 3 = 8，5 * 3 = 15
  ```
  
  3.**函数结果调用**
  
  ```javascript
  function getFullName(fist,second){
      return fist+second;
  }
  const fullNamestr =`全名是${getFullName("小","牟")}`
  ```
  
  > 字符串相加  
  
  ```javascript
  const str = '蓝山工作室';
  const num = 888;
  const boolean = true;
  const nullVar = null;
  const undefinedVar = undefined;
  
  console.log(str + num); 
  console.log(str + boolean);
  console.log(str + nullVar);
  console.log(str + undefinedVar)
  ```
  
  <img src ="https://cdn.jsdelivr.net/gh/qinyso/pic-bed/%E5%9B%BE%E7%89%872.png"></img>
  
  ## 拓展运算符

- 拓展运算符用三个连续的点（`...`）表示，核心作用是 **“展开” 可迭代对象（数组、字符串、类数组、对象等）的元素 / 属性
  
  1. **展开数组**

```javascript
const arr1=[1,2,3];
const arr2=[...arr1,4,5];//1,2,3,4,5
```

​         2.**给数组传参**

```javascript
function add(a,b,c,d){
 return a+b+c+d;
}
const num=[1,2,3,4]
const result= add(...num)
```

​      3.**浅拷贝(仅复制对象 / 数组的表层结构,引用嵌套结构)数组和对象**

```javascript
const obj ={
    name="省君语"
    hobby:{ indoor:"conding",outdoor:"codingoutdoor"}
};
const shallowCopy ={...obj};
shallowCopy.name="沈君玉";
console.log(obj.name);//省君语(表层是复制过来的,修改表层不影响原来的变量)
shallowCopy.hooby.indoor="看番";
console.log(obj.hobby.indoor);//看番(原对象被修改)
```

## 解构赋值

> [!NOTE]
> 
> 解构赋值是 ES6 引入的语法糖，核心作用是**从数组或对象中快速提取数据，并赋值给变量**，替代传统的 “逐个取值” 写法，让代码更简洁、可读性更强。

##### 1.**数组解构**

```javascript
const drink = ["珍珠", "椰果", "奶盖"];
const topping1 = drink[0];
const topping2 = drink[1];
const topping3 = drink[2];
console.log(topping1, topping2, topping3); // 珍珠 椰果 奶盖
// 解构赋值
const [z, y, g] = drink; 
console.log(z, y, g); // 珍珠 椰果 奶盖
```

如果你只对数组中的某些元素感兴趣，你可以忽略其他的元素：

```javascript
const[,,thirdElement]=[1,2,3,4,5];
console.log(thirdElement);//输出3
```

当你不确定数组的长度，但想要提取除前几个元素之外的所有元素时，可以使用剩余参数:

```javascript
const[fist,second,...rest]=[1,2,3,4,5];
console.log(first); // 输出: 1
console.log(second); // 输出: 2
console.log(rest); // 输出: [3, 4, 5]
```



##### **2.对象解构**

```javascript
const student = {name:"小秦",age="19",class="四班"}
const studentName = student.name;
const studentClass = student.class;
console.log(studentName, studentClass); 
//解构赋值
const {name,class:className}=student;
console.log(name,className);//小秦 四班
```

##### 3.解构默认值

```javascript
//数组默认值
const[pearl,coconut,cream ="仙草"]=["珍珠"]
console.log(pear,coconut,cream);//珍珠 undefined 仙草
//对象默认值
const { name, age = 16, class: className = "一班" } = { name: "小红" };
console.log(name, age, className); // 小红 16 一班

```

##### **4.函数参数解构**

```javascript
function print([x,y]){
    console.log(`x:${x},y:${y}`);
}
print([3,4])//3,4
```

## 作用域

> [!NOTE]
> 
> 就像咱们生活里的 “地盘规矩”—— 不同的代码片段有自己的 “**专属地盘**”，变量在哪个地盘定义，就只能在哪个地盘（及它的 “小地盘”）里用，出了地盘就 “认不到” 啦！**(≧ω≦)**
> 
> 作用域分为**全局作用域**和**局部作用域** 

```javascript
function counter(x,y){
    const s =x+y;
    console.log(s)//18
}
count(10,8)
console.log(s)//报错 (｡•́︿•̀｡)
```

1. 函数内部声明的变量，在函数外部无法被访问
2. 函数的参数也是函数内部的局部变量
3. 不同函数内部声明的变量无法互相访问
4. 函数执行完毕后，函数内部的变量实际被清空了

##### 块作用域

 简单说，块作用域就是用 `{ }`给变量划了块 “专属领地”

```javascript
{
    let age =18;
    console.log(age);//正常
}
console.log(age);//报错，超出了a的作用域
let flag =true;
if (flag){
    let str ="hello world";
    console.log(str)；//正常
}
console.log（str）;//报错 (｡•́︿•̀｡)
```

##### 全局作用域

全局作用域是程序中最外层的作用域，在该作用域内声明的变量、函数等，可被程序中所有代码（包括嵌套在其他作用域内的代码）访问和使用，其生命周期与程序运行周期一致，程序启动时创建，结束时销毁。

```javascript
<script>
  // 此处是全局

  function sayHi() {
    // 此处为局部
  }

  // 此处为全局
</script>
```

在全局作用域声明的变量，在其他作用域都可以被访问

```javascript
<script>
    const name ="沈君玉"//全局作用域
    //在函数作用域里访问全局
    function sayHi(){
    console.log('你好'+name)
}
// 全局变量 flag 和 x
    const flag = true
    let x = 10

      // 块作用域中访问全局
    if(flag) {
      let y = 5
      console.log(x + y) // x 是全局的
    }
</script>
```

1. 为 `window` 对象动态添加的属性默认也是全局的，不推荐！
2. 函数中未使用任何关键字声明的变量为全局变量，不推荐！！！
3. 尽可能少的声明全局变量，防止全局变量被污染！
   
   

> 了解Javascript作用域有助于规范代码书写习惯，避免因作用域导致的语法错误(≧∇≦)ﾉ

##### 作用域链

ES6 的作用域链可以理解为：代码在查找变量时，会从当前作用域开始，逐层向上搜索父级作用域，直到全局作用域，形成一条链式查找路径。

```javascript
let globalVar ="全局变量"
function outer(){
    //外层函数作用域
   function innner(){
    //内层函数作用域
    let innerVar ="内层变量"
    // 查找变量：先找自己的作用域（找到innerVar）
    console.log(innerVar);
    // 自己没有，向上找外层作用域（找到outerVar）
    console.log(outerVar); 
    // 外层也没有，继续向上找全局作用域（找到globalVar）
    console.log(globalVar); 
}
     inner();
}
 outer();
```

> 作用域链的核心是**变量查找只向上不向下**，内层作用域可以访问外层及全局变量，但外层无法访问内层变量。

1. 嵌套关系的作用域串联起来形成了作用域链
2. 相同作用域链中按着从小到大的规则查找变量
3. 子作用域能够访问父作用域，父级作用域无法访问子级作用域
   
   

##### 闭包

简单说就是内层函数引用了外层函数的变量，且内层函数被外部访问时，外层函数的作用域不会被销毁，从而保留了对这些变量的访问能力。

> 使用闭包函数可以创建隔离作用域避免全局变量污染。(´･ᴗ･`)

```javascript
function fun(){
    let num = 0;
    function add(){
    num++
console.log(num)
}
return add
}
let addFun()=fun()
addFun()//1
addFun()//2
addFun()//3
```

这个例子中，我们在`fun`函数里又定义了`add`函数，并将`add`函数返回给全局上下文进行使用。

正常情况下，函数执行完成，内部变量会被销毁（释放内存空间）。

通过打印结果可以看到，当`fun`函数执行完成之后，其内部的变量`num`本该跟着函数执行完成一起被清除，但`fun`函数返回的`add`函数赋值给`addFun`后，`addFun`内部可以却访问之前`fun`函数定义的`num`，但全局作用域无法访问。

这就形成了一个闭包。当一个函数`a`内部声明了另一个函数`b`，函数`b`中使用了函数`a`中声明的变量或者函数，并将函数`b`赋值给全局变量或者作为返回值传给全局变量，就形成了闭包。

*简单说就是：内层函数引用外层变量，且被外部持有，让外层变量 “超期保鲜” 的组合～ (≧∇≦)ﾉ*

###### 特点

1. 可以引用外部函数的作用域
2. 可以创建私有作用域，且不会被回收内存。
3. 容易导致内存泄露

##### 箭头函数

箭头函数是 ES6 引入的函数简写语法，允许使用“箭头”`=>`定义函数，箭头函数等价于传统匿名函数。

括号内表达传入的参数，如果只有一个参数，括号可以省略，如果有多个参数或没有参数，则不能省略。

箭头后面为函数体，用大括号包裹。如果只含返回值，大括号可以省略。

```javascript
let f = () => 5;
//等同于
let f = function（）{return 5}
//等同于
let sum =(num1,num2)=>num1+num2
//等同于
le sum=function(num1,num2){
    return num1+num2;
};
//只有一个参数，可以忽略小括号
cont fn = x=>{
    cosole.log(x);
}
fn(1)
//只有一行代码时，可以忽略大括号
constb fn =x=> console.log(x)
fn(1)
//只有一行代码的时候，可以忽略return
const fn =x=>x+x
console.log(fn(1))
```

#### this指向

> 上一节课有讲过，这里做一点拓展  ฅ^•ﻌ•^ฅ

在 JavaScript 中，`this`是一个动态绑定的关键字，其指向由函数的**调用方式**决定

##### 1、默认绑定规则

- ##### 默认绑定全局对象window


```javascript
console.log(this === window);   //true
```

- ##### 全局作用域下独立调用函数，this指向window


```javascript
   function test() {
      console.log(this===window);  //true
    }
    test();
```

- ##### 全局函数就是全局对象的方法。


```javascript
window.fun()
```

#### 2、隐式绑定规则

谁调用就指向谁

- ##### 对象内调用


```javascript
let obj ={
    name:'obj',
    fooo:function(){
      console.log(this);//this指向obj
    }
}
      obj.foo()
```

- ##### 再加一层函数


```javascript
  let obj = {
      name: 'obj',
      foo: function () {
        console.log(this);    //obj
        function test() {
          console.log(this);  //window  为什么？  因为test独立调用
        }
        test()
      }
    }
    obj.foo()
```

- ##### 隐式丢失


```javascript
let obj ={
    name:'obj'
    foo:function(){
    condole.log(this);
}
}
let bar=obj.foo
bar()
```

<img  src="https://cdn.jsdelivr.net/gh/qinyso/pic-bed/20251107200732833.png" ></img>

- ##### 箭头函数中的this


不同于普通函数箭头函数不会创建自己的 `this`，它的 `this` 是**继承自外层作用域**（即定义时的上下文），且一旦确定就不会改变。

   

```javascript
//普通函数
let obj1 ={
    hp:555,
    sayHp:function(){
        console.log(this.hp)
    }   
}
obj1.sayHp()//555
//箭头函数
let obj2 ={
    hp:555,
    sayHp:()=>{
        console.log(this.hp)
    }
}
obj2.sayHp//undefined (｡•́︿•̀｡)
```

- ##### 使用bind/call/apply显式绑定外部函数的this


##### call:

- ##### call可以立即调用并执行函数

```javascript
function fun(){
    console.log*("蓝山工作室")
}
fun.call()//蓝山工作室
```

- ##### call可以改变函数中this的指向：

```javascript
var a =0
let obj ={
    a:1,
    foo:foo 
}
function foo(){
    console.log(this);//指向obj
    function test(){
         console.log(this);
    }
    test.call(obj)
}
obj.foo()
```

- ##### 考虑到传参

```javascript
  const dog ={
      name="蓝妹"
      eat(food){
          console.log(`我是${this.name},我要${this.food}`)
      }
  }
  const cat ={
      name ="邮宝"
  }
  dog.eat(“骨头”)
  dog.eat.call(cat,'鱼')
```

#####     apply:

- ##### call 和apply的区别就在于传参的方式不同：

```javascript
const dog = {
  name: '邮宝',
  eat (food, hobby) {
    console.log(`我是${this.name}，我要吃${food}，喜欢${hobby}`)
  }
}

const cat = {
  name: '蓝妹'
}

dog.eat.apply(cat,['鱼，’睡觉'])// 我是蓝妹，我要吃鱼，喜欢睡觉

```

##### bind:

- **作用**：**不立即调用函数**，而是返回一个新的函数（绑定了 `this` 的函数副本），后续调用该新函数时，`this` 始终指向绑定的对象。

```javascript
onst dog = {
  name: '邮宝',
  eat (food, hobby) {
    console.log(`我是${this.name}，我要吃${food}，喜欢${hobby}`)
  }
}

const cat = {
  name: '蓝妹'
}
const fun=dog.eat.bind(cat,'鱼'，'睡觉')//什么没打印
fun()// 我是蓝妹，我要吃鱼，喜欢睡觉
```

------

#### 字符串方法

<img src="https://cdn.jsdelivr.net/gh/qinyso/pic-bed/%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE.png"></img>

#### 1.获取字符串长度


```javascript
const str ='Hello';
str.length//输出5
```

#### 2.获取字符串指定位置的值

- charAt()和charCodeAt()方法都可以通过索引来获取指定位置的值：

- charAt() 方法获取到的是指定位置的字符；

- charCodeAt()方法获取的是指定位置字符的Unicode值。

##### charAt() 方法可以返回指定位置的字符。其语法如下

```javascript
string.chatAt(index)
```
##### index表示字符在字符串中的索引值：

```javascript
const str = 'hello';
str.charAt(1)  // 输出结果：e
```
>那么它和字符串索引获取对应字符有什么区别吗？

```javascript
const str = 'hello';
str.charAt(1)  // 输出结果：e 
str[1]         // 输出结果：e 
str.charAt(5)  // 输出结果：'' 
str[5]         // 输出结果：undefined
```

可以看到，当index的取值不在str的长度范围内时，str[index]会返回**undefined**，而charAt(index)会返回**空字符串**；

##### charCodeAt()

- charCodeAt()：该方法会返回指定索引位置字符的 **Unicode 值**，返回值是 0 - 65535 之间的整数，表示给定索引处的 UTF-16 代码单元，如果指定位置没有字符，将返回 **NaN**：


```javascript
let str ='abcdefg'
console.log(str.charCodeAt(1));//"b" (98)
```

#### 3. 检索字符串是否包含特定序列

- 这5个方法都可以用来检索一个字符串中是否包含特定的序列。其中前两个方法得到的指定元素的索引值，并且只会返回第一次匹配到的值的位置。后三个方法返回的是布尔值，表示是否匹配到指定的值。


> [!NOTE]
>
> 注意：这5个方法都**对大小写敏感**！**⊙ω⊙**

##### （1）indexOf()

- indexOf()：查找某个字符，**有则返回第一次匹配到的位置**，否则返回-1，其语法如下：

```javascript
string.indexof(searchvalue,fromindex)
```

- **searchvalue：**必需，规定需检索的字符串值

- **fromindex：**可选的整数参数，规定在字符串中开始检索的位置。它的合法取值是 0 到 string.length - 1。如省略该，则从字符串的首字符开始检索。

```javascript
let str ="abcdefgabc"
console.log(str.indexOf("a"))// 输出结果：0
console.log(str.indexOf("z"))  // 输出结果：-1
console.log(str.indexOf("c",4))// 输出结果：9
```

##### （2）lastIndexOf()

- lastIndexOf()：查找某个字符，有则返回最后一次匹配到的位置，否则返回-1

```javascript
let str = "abcabc";
console.log(str.lastIndexOf("a"));  // 输出结果：3
console.log(str.lastIndexOf("z"));  // 输出结果：-1
```

- 该方法和indexOf()类似，只是查找的顺序不一样，indexOf()是**正序查找**，lastIndexOf()是**逆序查找**。

##### （3）includes()

**includes()**：该方法用于判断字符串是否包含指定的子字符串。如果找到匹配的字符串则返回 true，否则返回 false。该方法的语法如下：

```javascript
string.includes(searchvalue,start)
```

- searchvalue：必需，要查找的字符串；
- start：可选，设置从那个位置开始查找，默认为 0。

```javascript
let str = 'Hello world!';

str.includes("o")// 输出结果：true
str.includes('z')  // 输出结果：false
str.includes('e', 2)  // 输出结果：false
```

#### 4. 连接多个字符串

- concat() 方法用于连接两个或多个字符串。该方法不会改变原有字符串，会返回连接两个或多个字符串的新字符串。其语法如下：

```Javascript
string.concat(string1,string2,...,)
```

- 其中参数 string1, string2, ..., stringX 是必须的，他们将被连接为一个字符串的一个或多个字符串对象。

```javascript
let str="abc"
console.log(str.concat("efg"))//输出结果："abcefg"
console.log(str.concat("efg","hijk"));//输出"abcefghijk"

```

>虽然concat()方法是专门用来拼接字符串的，但是在开发中使用最多的还是加操作符+，因为其更加简单。

#### 5. 字符串分割成数组

- split() 方法用于把一个字符串分割成**字符串数组**。该方法不会改变原始字符串。其语法如下：

```javascript
string.split(separator,limit)
```

该方法有两个参数：

- separator：**必需**。字符串或正则表达式，从该参数指定的地方分割 string。
- limit：**可选**。该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。

```javascript
let str = "abcdef";
str.split("c");    // 输出结果：["ab", "def"]
str.split("",4)  // 输出结果：['a', 'b', 'c', 'd'] 
```

- 其实在将字符串分割成数组时，可以同时拆分多个分割符，使用正则表达式即可实现：

```javascript
const list ="apple,banana,cherry"
const frut=list.split(/[,;]/)
console.log(fruit);// 输出结果：["apple","banana","cherry"]
```

#### 6. 截取字符串

- substr()、substring()和 slice() 方法都可以用来截取字符串。

##### slice()

- slice() 方法用于提取字符串的某个部分，并以新的字符串返回被提取的部分。其语法如下：

```javascript
string.slice(start,end)
```

该方法有两个参数：

- start：必须。 要截取的片断的起始下标，第一个字符位置为 0。如果为负数，则从尾部开始截取。
- end：可选。 要截取的片段结尾的下标。若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串。如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。

> *上面说了，如果start是负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推：*

```javascript
let str = "abcdefg";
str.slice(1,6);   // 输出结果："bcdef" 
str.slice(1);     // 输出结果："bcdefg" 
str.slice();      // 输出结果："abcdefg" 
str.slice(-2);    // 输出结果："fg"
str.slice(6, 1);  // 输出结果：""
```

>注意，该方法返回的子串**包括开始处的字符**，但**不包括结束处的字符**。

#####  substr()

- substr() 方法用于在字符串中抽取从开始下标开始的指定数目的字符。其语法如下：

```javascript
string.substr(start.length)
```

该方法有两个参数：

- start	必需。要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。
- length：可选。子串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。

```javascript
let str = "abcdefg";
str.substr(1,6); // "bcdefg" 
str.substr(1);   // "bcdefg" 相当于截取[1,str.length-1]
str.substr();    // "abcdefg" 相当于截取[0,str.length-1]
str.substr(-1);  // "g"

```

#####  substring()

substring() 方法用于提取字符串中介于两个指定下标之间的字符。其语法如下：

```javascript
string.substring(from ,to)
```

该方法有两个参数：

- from：必需。一个非负的整数，规定要提取的子串的第一个字符在 string 中的位置。

- to：可选。一个非负的整数，比要提取的子串的最后一个字符在 string 中的位置多 1。如果省略该参数，那么返回的子串会一直到字符串的结尾。

  
  > 如果参数 from 和 to **相等**，那么该方法返回的就是一个**空串**（即长度为 0 的字符串）。如果 **from 比 to 大**，那么该方法在提取子串之前会先**交换**这两个参数。并且该方法**不接受负的参数**，如果参数是个负数，就会返回这个字符串。

#### 7. 字符串大小写转换

- toLowerCase() 和 toUpperCase()方法可以用于字符串的大小写转换

##### toLowerCase()

```javascript
let str = "adABDndj";
str.toLowerCase();//输出"adabdndj"
```

##### toUpperCase()

- toUpperCase()：该方法用于把字符串转换为大写。

```javascript
let str = "adABDndj";
str.toUpperCase(); // 输出："ADABDNDJ"
```

- 我们可以用这个方法来将字符串中第一个字母变成大写：

```javascript
let word="apple"
word =word[0].toUpperCase()+word.substr(1)
console.log(word)) // 输出结果："Apple"
```

#### 8.字符串模式匹配

replace()、match()和search()方法可以用来匹配或者替换字符。

##### replace()

- replace()：该方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。其语法如下：

```javascript
string.replace(searchvalue,newvalue)
```

该方法有两个参数：

该方法有两个参数：

- searchvalue：必需。规定子字符串或要替换的模式的 正则表达式象。如果该值是一个字符串，则将它作为要检索的直接量文本模式，而不是首先被转换为 正则表达式。
- newvalue：必需。一个字符串值。规定了替换文本或生成替换文本的函数。

>只匹配第一个匹配的字符串

##### 2）replaceAll

在 JavaScript 中，`replaceAll()`方法用于将字符串中的所有匹配项替换为另一个字符串。

**语法**：

```
str.replaceAll(searchValue, replaceValue)
```

- searchValue：要被替换的字符串或正则表达式。如果是字符串，则会被严格匹配进行替换。如果是正则表达式，需要注意正则表达式的特殊字符和匹配规则。
- replaceValue：用于替换的字符串。

```javascript

const str = "The quick brown fox jumps over the lazy dog. The fox is brown.";

// 将所有的 "fox" 替换为 "cat"
const newStr = str.replaceAll("fox", "cat");
console.log(newStr); 
// 输出：The quick brown cat jumps over the lazy dog. The cat is brown.

```

**注意**，在一些旧版本的浏览器中可能不支持 replaceAll()方法。在这种情况下，可以使用正则表达式结合 replace()方法来实现类似的功能：

```javascript
const str = "The quick brown fox jumps over the lazy dog. The fox is brown.";
const newStr = str.replace(/fox/g, "cat"); 
console.log(newStr); 
// 输出：The quick brown cat jumps over the lazy dog. The cat is brown.

```

#### 9.移除字符串收尾空白符

- trim()、trimStart()和trimEnd()这三个方法可以用于移除字符串首尾的头尾空白符，空白符包括：空格、制表符 tab、换行符等其他空白符等。

##### trim()

trim() 方法用于移除字符串**首尾**空白符，该方法不会改变原始字符串：

```javascript
let str =" abcdef"
str.trim() // 输出结果："abcdef"
```

注意，该方法不适用于null、undefined、Number类型。

##### trimStart()

trimStart() 方法的的行为与trim()一致，不过会返回一个**从原始字符串的开头删除了空白的新字符串**，不会修改原始字符串：

```javascript
const str = '  hello world  ';
console.log(str.trimStart())// 'hello world  '（仅开头空格被删除，结尾空格保留）
```

##### trimEnd()

trimEnd() 方法的的行为与trim()一致，不过会返回一个**从原始字符串的结尾删除了空白的新字符串**，不会修改原始字符串：

```javascript
const s = '  abc  ';
s.trimEnd()   // "  abc"
```

#### 10.获取字符串本身

**valueOf()**和**toString()**方法都会返回字符串本身的值，感觉用处不大。

#### 11.重复一个字符串

repeat() 方法返回一个新字符串，表示将原字符串重复n次

```javascript
'x'.repeat(3) // 输出结果："xxx"
'hello'.repeat(2)// 输出结果："hellohello"
'na'.repeat(0) // 输出结果：""
```

如果参数是小数，会向下取整：

```javascript
'na'.repeat(2.9) // 输出结果："nana"
```

如果参数是负数或者Infinity，会报错：

```javascript
'na'.repeat(Infinity)   // RangeError
'na'.repeat(-1)         // RangeError
```

如果参数是 0 到-1 之间的小数，则等同于 0，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于-0，repeat视同为 0。

```javascript
'na'.repeat(-0.9)   // 输出结果：""
```

如果参数是NaN，就等同于 0：

```javascript
'na'.repeat(NaN)    // 输出结果：""
```

如果repeat的参数是字符串，则会先转换成数字。

```javascript
'na'.repeat('na')   // 输出结果：""
'na'.repeat('3')    // 输出结果："nanana"
```
>字符串方法还有很多，这里就不一一介绍了

#### **Promise**

所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

promise对象有以下两个特点。

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。

下面代码创造了一个`Promise`实例。

```javascript
const promise =new Promise(function(resolve,reject){
    //
    if(/*异步操作成功*/){
        resolve(value)
    }
    else{
        reject(error);
    }
})
// 使用then方法处理Promise的结果
myPromise.then((result) => {
  console.log('操作成功:', result);
}).catch((error) => {
  console.log('操作失败:', error);
});
```

- **then()** ：用于处理异步操作成功的情况。

- **catch()** ：用于处理异步操作失败的情况。

>这里只简单介绍promise,关于异步的部分没看懂没关系，后期还会详细介绍

举一个使用fecth返回promise对象的网络请求例子，方便大家理解Promise的作用，暂时只需了解即可

```javascript
// 发送 GET 请求获取用户数据
fetch('https://jsonplaceholder.typicode.com/users/1') // 测试接口
  .then(response => {
    // 先判断响应是否成功（状态码 200-299）
    if (!response.ok) {
      throw new Error(`请求失败，状态码：${response.status}`);
    }
    // 解析响应体为 JSON 格式（返回一个新的 Promise）
    return response.json();
  })
  .then(userData => {
    // 成功获取数据后处理
    console.log('用户数据：', userData);
    console.log('用户名：', userData.name);
  })
  .catch(error => {
    // 捕获请求过程中的所有错误（网络错误、解析错误等）
    console.error('请求出错：', error.message);
  });
```

#### Module

命令

- **export**：规定模块对外接口
  - **默认导出**：`export default Person`(导入时可指定模块任意名称，无需知晓内部真实名称)
  - **单独导出**：`export const name = "Bruce"`
  - **按需导出**：`export { age, name, sex }`(推荐)
  - **改名导出**：`export { name as newName }`
- **import**：导入模块内部功能
  - **默认导入**：`import Person from "person"`
  - **整体导入**：`import * as Person from "person"`
  - **按需导入**：`import { age, name, sex } from "person"`
  - **改名导入**：`import { name as newName } from "person"`
  - **自执导入**：`import "person"`
  - **复合导入**：`import Person, { name } from "person"`
- **复合模式**：`export命令`和`import命令`结合在一起写成一行，变量实质没有被导入当前模块，相当于对外转发接口，导致当前模块无法直接使用其导入变量
  - **默认导入导出**：`export { default } from "person"`
  - **整体导入导出**：`export * from "person"`
  - **按需导入导出**：`export { age, name, sex } from "person"`
  - **改名导入导出**：`export { name as newName } from "person"`
  - **具名改默认导入导出**：`export { name as default } from "person"`
  - **默认改具名导入导出**：`export { default as name } from "person"`

继承：`默认导出`和`改名导出`结合使用可使模块具备继承性

设计思想：尽量地静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量

**严格模式**：ES6 的严格模式（Strict Mode）是一种 JavaScript 的运行模式，通过限制一些不规范的语法和行为，让代码更严谨、更安全，减少运行时错误。ES6模块自动采用严格模式(不管模块头部是否添加`use strict`)。

>坚持一下下，马上就要看完了(≧ω≦)

## Iterator（遍历器）

迭代器是 ES6 中引入的一种接口机制，用于统一各种数据结构的遍历方式（如数组、对象、Map 等），核心是通过 `next()` 方法逐个返回数据。

terator 的遍历过程是这样的。

（1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。

（2）第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。

（3）第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。

（4）不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。

每一次调用`next`方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，表示遍历是否结束。

- 迭代器对象方法：
- **next()** ：下一步操作，返回`{ value,done }`(必须部署)
- **return()** ：`for-of`提前退出调用，返回`{ done: true }`
- **throw()** ：不使用，配合`Generator函数`使用

#### 下面是一个模拟`next`方法返回值例子

```javascript
var it =makeIterator(['a','b']);
it.next()
it next()
it.next() 
//创建一个迭代器（Iterator）对象，用于按顺序遍历传入数组中的元素。
function makeIterator(array){
    var nextIindex =0;// 记录当前遍历的位置（索引）
    return{
        next:function(){// 返回一个包含 next 方法的对象（迭代器对象）
            return nextIndex<array.length?// 判断是否还有未遍历的元素
                 // 有元素：返回当前元素值 + done: false（未完成）
                {value:array[nextIndex++],done:false}:
                 // 无元素：返回 undefined + done: true（已完成）
                {value:undefined,done:true};
        }
    };
}
```

   通俗易懂的说就是，迭代器是一种对象，它定义了一种用于遍历集合（如数组、对象等）的方法。迭代器使得可以按顺序访问集合中的元素，而无需了解集合的内部结构。

- `forEach` 是数组的专用方法，只能用于数组。
- `for...in` 可以用于遍历数组和对象的属性，但不能保证属性的遍历顺序。
- `forEach` 通常用于数组元素的遍历，而 `for...in` 更适用于遍历对象的属性

##### forEach

- `forEach` 是数组的高阶函数，用于遍历数组中的每个元素。它接受一个回调函数作为参数，该回调函数会在数组的每个元素上执行

```javascript
const fruit =['苹果'，'香蕉'，'橙子']
//foreach遍历数组
fruits.forEach((item,item)=>{
    console.log(`索引${index}:${item}`);
})

// 输出：
// 索引 0：苹果
// 索引 1：香蕉
// 索引 2：橙子
```

##### For in

- `for...in` 循环用于遍历一个对象的所有可枚举属性。它可以用来遍历数组和对象。

```javascript
const person = {
    name:'张三'，
    age:'20',
    gender:'男'
}；
//// for...in 遍历对象属性
for（const key in person）{
    console.log(`${key}:${person[key]}`);// 通过 key 访问属性值
}

// 输出：
// name：张三
// age：20
// gender：男
```

- ##### for of

`or...of`循环用于遍历可迭代对象（例如数组、字符串、Set、Map等），它会迭代对象中的每个元素并执行指定的代码块。

```javascript
const fruits = ['苹果', '香蕉', '橙子'];
//for ...of 直接获取数组元素
for(const fruit of fruits){
    console.log(fruit);
    //支持break中断
    if(fruit ==='香蕉') break;
}
// 输出：
// 苹果
// 香蕉（遇到 break 停止）
```

