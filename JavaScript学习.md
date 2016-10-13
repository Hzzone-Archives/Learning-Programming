##JavaScript学习
<br>

----
####对象
>JavaScript的对象是一种无序的集合数据类型，它由若干键值对组成。

```JavaScript
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
```
***最后一个键值对不需要在末尾加,***   
JavaScript可以自由地给一个对象添加或删除属性:
```JavaScript
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
xiaoming.age; // undefined
delete xiaoming['name']; // 删除name属性
xiaoming.name; // undefined
delete xiaoming.school; // 删除一个不存在的school属性也不会报错
```

检测xiaoming是否拥有某一属性，可以用`in`操作符:
```JavaScript
'name' in xiaoming; // true
'grade' in xiaoming; // false
```
<span style="color: red">JavaScript对象的所有属性都是字符串，不过属性对应的值可以是任意数据类型</span>    
***所有对象都继承object类，都有toString属性*** 
判断一个属性是否是xiaoming自身拥有的，而不是继承得到的，可以用hasOwnProperty()方法：  
```JavaScript
var xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```
<br>

----
<br>
####Map和set
初始化`Map`需要一个二维数组,例如：
```JavaScript
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
```
也可以直接初始化一个空的map,例如：
```JavaScript
var m = new Map(); // 空Map
```
**Map的一些方法**  
```JavaScript
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 得到value
m.delete('Adam'); // 删除key 'Adam'
```
Map与对象属性存储数据的区别：
>object通过属性（也就是名字）来访问对应的成绩，而new Map()构建了一个属于Map类的一个实例对象，这个对象内部有一个map（也就是一系列的键值对），通过对象的get方法访问这个map，从而获取成绩。所以说，两者在实现上完全不同，一个通过属性访问，一个通过方法访问map从而达到目的。另外，map支持不是字符串的key，比如数字这些，而object的key只能是字符串。

<br>
`Set`是一组不存储value的key的集合，而且key不能重复。重复也没有任何效果。    
创建一个`Set`，需要提供一个Array作为输入，或者直接创建一个空`Set`,并且重复元素将被过滤:
```JavaScript
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
//注意数字3和字符串'3'是不同的元素
```
**Set的一些方法**  
```JavaScript
s.add(4);//add(key)方法可以添加元素到Set中，可以重复添加，但不会有效果
s.delete(3);//通过delete(key)方法可以删除元素
```

----
####Iterable
`Array`、`Map`和`Set`都属于`iterable`类型，具有`iterable`类型的集合可以通过for ... of循环来遍历,例如：
```JavaScript
        var arr = [1, 2, 3];
        for(var x off arr){
            alert(x);
        }
```

######对Iterable的遍历，forEach方法，参数为一个函数。
```JavaScript
        var a = ['A', 'B', 'C'];
        a.forEach(function (element, index, array) {
        // element: 指向当前元素的值
        // index: 指向当前索引
        // array: 指向Array对象本身
            alert(element);
        });
        var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
        m.forEach(function(key, value, map){

            alert(key);
            alert(value);
        });
        var s = new Set([1, 2, 3]);
        s.forEach(function(element, sameElement, set){
            alert(element);
            alert(sameElement);
            alert(set);
        });
```
***forEach***方法的某些参数也可以省略，例如遍历一个数组：
```JavaScript
        var a = ['A', 'B', 'C'];
        a.forEach(function (element) {
            alert(element);
        });
```
>***forEach***方法参数名字不固定，但是位置是固定的，如果只关心element，那么给forEach一个参数就可以，如果需要index，那么就要给两个参数，如果需要array，就要给三个，也就是这三个参数的含义是定好的。

----
####JavaScript函数
形如：
```JavaScript
function abs(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```
JavaScript可以传入多或者少参数，不会有影响
```JavaScript
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
abs(); // 返回NaN
```
***arguments***参数只在函数内部起作用，可以获得传入的所有参数，也就是说，即使函数不定义任何参数，还是可以拿到参数的值：
```JavaScript
function abs() {
    if (arguments.length === 0) {
        return 0;
    }
    var x = arguments[0];
    return x >= 0 ? x : -x;
}

abs(); // 0
abs(10); // 10
abs(-9); // 9
```
***arguments***最常用于判断传入参数的个数:
```JavaScript
// foo(a[, b], c)
// 接收2~3个参数，b是可选参数，如果只传2个参数，b默认为null：
function foo(a, b, c) {
    if (arguments.length === 2) {
        // 实际拿到的参数是a和b，c为undefined
        c = b; // 把b赋给c
        b = null; // b变为默认值
    }
    // ...
}
```
***rest***参数可以直接获得多余的参数：
***rest***为一个数组
```JavaScript
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```

----
####变量作用域
* 内部函数变量从内到外，和外部函数有同名变量，会屏蔽外部函数的变量
* 内部函数可以访问外部函数变量
* JavaScript会将所以变量声明提到作用域最前面(可能类似于C语言)
* 声明的全局变量会被绑定到 ***window*** 对象中，成为他的一个属性(函数也是一个全局变量)    
例如：
```JavaScript
var course = 'Learn JavaScript';
alert(course); // 'Learn JavaScript'
alert(window.course); // 'Learn JavaScript'

function foo() {
    alert('foo');
}
foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用

window.alert('调用window.alert()');
```
* for循环中的i不是只在for中去作用，可以用关键字 ***let***替代var可以申明一个块级作用域的变量： 
```JavaScript
function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    i += 1; // SyntaxError
}
```
* JavaScript中使用 ***const*** 声明常量(常量一般全部用大写)
```JavaScript
const PI = 3.14;
PI = 3; // VM1774:1 Uncaught TypeError: Assignment to constant variable.
PI; // 3.14
```