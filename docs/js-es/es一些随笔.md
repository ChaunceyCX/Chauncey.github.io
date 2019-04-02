# ES基础
## ES各版本发展变化
ES版本|发布时间|新增特性
-|-|-
ES5|09-11|扩展了object\array\fouction的功能
ES6|15-6|类\模块化\箭头函数\函数参数默认值
ES7|16-3|incloudes\指数操作符
ES8|17-6|sync/await,object.values(),object.entries(),string padding

## ES6特性

1. 类(class)

```
class people {
    constructor(name,age){
        this.name=name;
        this.age=age;
    }
    //tostring 是原型对象的属性
    tostring() {
        console.log('name:' + this.name);
    }
}
var apeople = new people('xxx',18);
apeople.tostring();

//继承
class man extends people{
    constructor(sex){
        //子类必须super父类
        super('xxx',18);
        this.sex='male';
    }
}
```

2. 模块

ES6支持常量与变量的导出,ES6将一个文件视为一个模块

- 导出
    ```
    //a.js
    var name = 'xxx'
    const sqrt = Math.sqrt;
    //导出多个变量或者常量
    export {name,sqrt};
    ```
    导出函数
    ```
    //b.js
    export fuction add (num) {
        return num++;
    }
    ```
- 导入
    定义好的模块可以导入
    ```
    import {name,sqrt} from 'a';
    ```

3. 箭头函数

箭头函数不仅是function的缩写,箭头函数与包围它的代码共享同一个this,能很好的解决this的指向问题

- 箭头函数的结构
    箭头函数之前是一个控括号,单个参数名或者多个参数,箭头函数后面可以是一个表达式作为函数的返回值,或者是花括号括起来的函数体(return返回值或者返回undefined)
    ```
    //箭头函数的例子
    ()=>1
    v=>v+1
    (a,b)=>a+b
    ()=>{
        alert("xxx")
    }
    f=>{
        return 0
    }
    ```
    >无论是箭头函数还是bind,每次执行后都会返回一个新的函数的引用,因此如果需要函数的引用做别的事情(譬如卸载监听器),那么你必须保存这个函数的引用

4. 函数参数默认值
   
   ```
   fun (a=1,b=2){

   }
   ```

5. 模板字符串
   使用模板字符串使得字符串的拼接变得更加简单,只需要将变量放到大括号中
   ```
   //不使用模板字符串
   var name = 'name:' +name
   //使用模板字符串
   var name = 'name: ${name}'
   ```
6. 解构赋值
   
   快速的将数组或者字符串中提出值赋值给变量中
- 