---
title: 前端面试题（一）
date: 2017-06-10 18:52:40
categories: 前端
tags: [前端]
---
<Excerpt in index | 首页摘要> 
前端面试题（一）
<!-- more -->
<The rest of contents | 余下全文>

-----
# javascript
### 1.运行结果
```javascript
function a(){
    //声明result是一个数组
    var result=new Array();
    for(var i=0;i<10;i++){//i=0-9
        //数组里面的0-9对应的元素是一个匿名回调，由于这个函数没有调用，所以不会返回里面的i值
        result[i]=function(){
            return i;
        }
    }
    //这里return了，result里面脚标0-9存放的是function匿名函数
    return result;
}
//funcs就是那个result
var funcs=a();
for(var i=0;i<funcs.length;i++){
    //将里面0-9脚标对应的元素打印出来，所以都是相同的那个匿名函数
    console.log(funcs[i]);
}
```
结果：
```javascript
function (){
        return i;
    }
```
将a函数做一些修改，我们让result后面的匿名函数执行
```javascript
function a(){
    var result=new Array();
    for(var i=0;i<10;i++){
        result[i]=(function(){
            return i;
        })()
    }
    return result;
}

var funcs=a();
for(var i=0;i<funcs.length;i++){
    console.log(funcs[i]);
}
```
结果
```
输出0-9
```

### 2. 问结果
```javascript
var func=function(){
    console.log("1");
    setTimeout(function(){
        console.log("2");
    },0);
    console.log("3");
}
func();
```
答案是：132
分析
因为setTimeout是异步函数，并不会等着他执行完毕在去执行下面的`console.log("3")`,所以答案是132而不是123.这里可以参考jQuery源码中的defferred延迟方法

### 3.for in与for of
```javascript
var person=["Admin","21","shangdong"];
//下面这个同样可以用于对象，返回的是键
for(var i in person){
    console.log(i);
    //读取的是脚标
    //0 1 2
}
//下面这个对象不适用，因为他的键不是0123脚标
for(var i of person){
    console.log(i);
    //读取的脚标对应的值
    //Admin 21 shangdong
}

var person1={"name":"李明","age":23,"sex":"男"};
//返回的是键
for(var i in person1){
    console.log(i)
}
//想法读取里面的值
for(var i in person1){
    console.log(i+":"+person1[i])
}
```

### 4.未解决
```javascript
function Foo(){
    getName = function(){
        alert(1)
    }
    return this;
}
Foo.getName=function(){
    alert(2)
}
Foo.prototype.getName=function(){
    alert(3)
}
var getName=function(){
    alert(4)
}
function getName(){
    alert(5)
}
//求下面的输出结果
// Foo.getName();//13行给重写了，弹出2

// getName();//弹出4。这个不懂，虽然他在上面，但是后面紧跟
// //着又写了一个同名的函数啊！
// Foo().getName();//1。不懂
// getName();//注释其他的单独调用时是4。不注释是1
//new Foo.getName();//2
// new Foo().getName();//3
// new new Foo().getName();//3
```

### 5.求运行结果
```javascript
<script>
    function Parent(){
        this.a=1;
        this.b=[1,2,this.a];
        this.c={demo:5};
        this.show=function(){
            console.log(this.a,this.b,this.c.demo);
        };
    }

    function Child(){
        this.a=2;
        this.change=function(){
            this.b.push(this.a);
            this.a=this.b.length;
            this.c.demo=this.a++;
        }
    }

    Child.prototype=new Parent();
    var parent = new Parent();
    var child1=new Child();
    var child2=new Child();
    child1.a=11;
    child2.a=12;

    //parent.show();//1 [1,2,1]  5
    //child1.show();//11 [1,2,1]  5
    //child2.show();//12 [1,2,1]  5

    child1.change();
    // child2.change();

    //parent.show();1   [1,2,1] 5
    child1.show(); //5 [1,2,1,11] 4 没有弄懂第一个数为什么是5。弄懂了，这他妈是个陷阱，注意代码this.c.demo=this.a++,赋值以后，自己也加了1
    // child2.show();
</script>
```

### 6. $(window).load and $(document).ready区别

[stack overflow](https://stackoverflow.com/questions/5182016/what-is-the-difference-between-window-load-and-document-ready)

### 7.
```javascript
    for(var i=0;i<10;i++){
        setTimeout(function(){
            console.log(i);
        },10)
    }
    //输出10个10
```

### 8. 0-100的数，能被3整除，输出Fizz，能被5整除，输出Buzz，能被3和5整除，输出FizzBuzz，其余输出本身
```javascript
    function print(num){
        var three=num%3,
            five=num%5,
            result;
            
            if(three==0){
                if(!(three || five)){
                    result="FizzBuzz";
                    return result;
                }
                result="Fizz";
                return result;
            }
            else if(five==0){
                if(!(three || five)){
                    result="FizzBuzz";
                    return result;
                }
                result="Buzz";
                return result;
            }
            else{
                return result=num;
            }
            console.log(result);
    }
```

### 9.
```javascript
    var yi=50,
    mo='bubu',
    tt1={
        value:'xjz'
    },
    tt2={
        value:'123'
    },
    tt3=tt2;

    function change(yi,mo,tt1,tt2){
        yi=yi*10;
        mo='momo';
        tt1=tt2;
        tt2.value='xiaojz';
    }

    change(yi,mo,tt1,tt2);

    console.log(yi);
    console.log(mo);

    console.log(tt1.value);

    console.log(tt2.value);

    console.log(tt3.value);
```