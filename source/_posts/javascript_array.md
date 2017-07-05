---
title: 数组 -- Javascript高级程序设计读书笔记
date: 2017/7/5
---

#### 在数组末尾添加数据
```
var colors = ["red", "blue", "green"];
colors[colors.length] = "black";
colors[colors.length] = "brown";
```

#### 数组join

```
var colors = ["red", "blue", "green"];
alert(colors.join(",");    //red.green.blue
alert(colors.koin("||");    //red||green||blue
```

#### 数组栈方法  
push(): 可以接受任意数量的参数，把他们逐个添加到数组末尾并返回修改后的长度  
pop(): 从数组末尾移除最后一项，减少数组的length的值，返回移除的项

```
var colors = new Array();
var count = colors.push("red", "green");
alert(count);    //2

count = colors.push("black");
alert(count);    //3

var item = colors.pop();
alert(item);    //"black"
alert(colors.length);    //2
```
#### 数组队列方法  
shift(): 移除数组中的第一项并返回该项，数组长度减1,。

```
var colors = new Array();
var count = colors.push("red", "green");
alert(count);    //2

count = colors.push("black");
alert(count);    //3

var item = colors.shift();
alert(item);    //"red"
alert(colors.length);    //2
```
unshift(): 在数组前端添任意个项，并返回新数组的长度。同时使用unshift(),pop()可以从相反方向模拟队列。

```
var colors = new Array();
var count = colors.unshift("red", "green");
alert(count);    //2

count = colors.unshift("black");
alert(count);    //3

var item = colors.pop();
alert(item);    //"green"
alert(colors.length);    //2
```

#### 排序方法  
sort(): 默认按字符串排序

```
var values = [0, 1, 5, 10, 15];
values.sort();
alert(values);    //0, 1, 10, 15, 5
```
sort()可以接收一个比较函数作为参数  
如果第一个参数位于第二个之前返回一个负数  
如果相等返回0  
如果第一个参数位于第二个之后则返回一个正数  

```
function compare(value1, value2) {
    if(value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}

var values = [0, 1, 10, 15, 5];
values.sort(compare);
alert(values);    //0, 1, 5, 10, 15
```
对于数值类型或者其valueOf()会返回数值类型的对象类型，可用如下方法

```
function compare(value1, value2) {
    return value2 - value1;
}
```

#### 操作方法  
concat(): 添加新的项,可以是简单的值或者数组

```
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);

alert(colors); //red,green,blue
alert(colors2); //red,green,blue,yellow,black,brown
```
slice(): 截取数组，如果只有一个参数，返回指定位置到数组末尾所有项。如果有两个参数返回起始位置到结束位置（不包括）的项。

```
var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);

alert(colors2); //green,blue,yellow,purple
alert(colors3); //green,blue,yellow
```
splice():  
删除：2个参数，要删除的第一项的位置和要删除的项数  
插入：指定位置插入任意数量的项，3个参数：起始位置，要删除的项数，要插入的项（要插入多各项，可以在传入第四第五和任意多项。）  
替换：在指定位置插入任意的项，插入的项数不必与删除的项数相等。，3个参数，起始位置、要删除的项数，要插入的项。 

```
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1);
alert(colors); //green.blue
alert(removed); //red 

removed = colors.splice(1,0,"yellow","orange");
alert(colors); //green,yellow,orange,blue
alert(removed); //返回空数组

removed = colors.splice(1,1,"red","purple");
alert(colors); //green,red,purple,orange,blue
alert(removed); //yellow
```
#### 位置方法  
indexOf()  
lastIndexOf()  
都接受两个参数，要查找的项，（可选）查找起点位置  
在比较的时候，会使用全等操作符

#### 迭代方法  
以下5个方法，每个方法都接收两个参数，要在每一项上运行的函数和（可选的）运行该函数的作用域对象-影响this的值。传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。  
every(): 如果该函数对每一项都返回true,则返回true  
filter(): 返回该函数会返回true的项组成的数组  
forEach(): 对数组中的每一项运行给定函数，没有返回值  
map(): 返回每次函数调用结果组成的数组  
some(): 如果该函数对任一项返回true,则返回true  

```
var number = [1,2,3,4,5,4,3,2,1];

var everyResult = numbers.every(function(item, index, array) {
    return  (item > 2);
});
alert(evveyResult);  //false


var someResult = numbers.some(function(item, index, array) {
    return  (item > 2);
});
alert(someResult);  //true


var filterResult = numbers.filter(function(item, index, array) {
    return  (item > 2);
});
alert(filterResult);  //[3,4,5,4,3]


var mapResult = numbers.map(function(item, index, array) {
    return  item * 2;
});
alert(mapResult);  //[2,4,6,8,10,8,6,4,2]


numbers.forEach(function(item, index, array) {
   //执行某些操作
});
```
#### 归并方法  
reduce()  
reduceRight()  
这两个方法都会迭代数组所有的项，然后构建一个最终返回的值。其中reduce()方法从数组的第一项开始，而reduceRight()从最后一项开始。  
两个函数接收4个参数：前一个值，当前值，项的索引和数组对象，这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数是数组的第二项  

```
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array) {
    return prev + cur;
});
alert(sum); //15

var sum2 = values.reduceRight(function(prev, cur, index, array) {
    return prev + cur;
});
alert(sum2); //15
```




