---
title: 关于if语句的优化
date: 2020-12-6 11:27:13
tags:
---

初学JavaScript的时候，判断习惯性的使用 if else 循环习惯性的使用for of。随着写的东西多了，自觉有些不够优雅。所以记录一下这方面代码的使用心得。

1. 三元运算符
  使用情况： 如果是根据一个值的ture false进行两种操作 ，可以使用三元运算符代替
```
if(a>2){
  a=a+2
}else{
  a=0
}
```
 三元
```
a=a>2?a+2:0
``` 
 2. 短路运算符
使用情况：根据一个值判断一个代码块是否执行

```
if(bol)}{
   this.doSomething
}
```
短路写法
```
bol&&this.doSomething
```

3. 函数内部改写
例子：
```
const calc = {
    run: function(op, n1, n2) {
        const result;
        if (op == "add") {
            result = n1 + n2;
        } else if (op == "sub" ) {
            result = n1 - n2;
        } else if (op == "mult" ) {
            result = n1 * n2;
        } else if (op == "div" ) {
            result = n1 / n2;
        }
        return result;
    }
}

calc.run("sub", 5, 3); //2
```

改写
```
const calc = {
    add : function(a,b) {
        return a + b;
    },
    sub : function(a,b) {
        return a - b;
    },
    mult : function(a,b) {
        return a * b;
    },
    div : function(a,b) {
        return a / b;
    },
    run: function(fn, a, b) {
        return fn && fn(a,b);
    }
}

calc.run(calc.mult, 7, 4); //28
```

4. 用键值对存储 
   比如我们要对一些窗口的显示隐藏进行管理
分开写逻辑的话很杂乱
例子：
```
  data(){
   dialog:{
    aDialog:true;
    bDialog:true;
    bDialog:true;
}
   dialogManage(dialogName,bool){
      this.dialog[dialogName]=bool

}
 
}
 
```
