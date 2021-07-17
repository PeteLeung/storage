# this
## 普通函数this:指向函数使用时所在的环境
```
//1.全局作用域
function f1(){
  console.log(this.a);
}
var a = 10;
f1();  //window.f1()  this->window

//2.对象
function f2(){
  console.log(this);
}
var obj = {
  f2:f2
};
obj.f2();  //this -> obj

//3.构造函数
function Person(name,age){ 
  this.name = name;
  this.age = age;
}
var person1 = new Person("张三",20); //this -> person1 指向当前实例

//4.call()和apply()改变this指向
function add(a,b){
  return this.a + this.b + c + d;
}
var obj = {a:10,b:20};
add.call(obj,30,40);  //10+20+30+40 = 100
add.apply(obj,[30,40]);  //10+20+30+40 = 100
```

## 箭头函数this:箭头函数没有自己的this, 它的this是继承而来; 默认指向在定义它时所处的对象(宿主对象)。
### 1.箭头函数可以方便地让我们在 setTimeout ,setInterval中方便的使用this。
```
let aLi = getElementsByTagName('li');
for(let i=0;i<aLi.length;i++){
  aLi[i].onclick = function(){
    setTimeout(function(){ //超时调用的代码都是在全局作用域中执行的，因此函数中this的值在非严格模式下指向window对象，在严格模式下是undefined
      console.log(this); //this -> window
    },1000);
  }
}
```
可以看到这里的this指向的是window对象，这是由于setInterval跟setTimeout调用的代码运行在与所在函数完全分离的执行环境上。这会导致这些代码中包含的 this 关键字会指向 window (或全局)对象。
```
let aLi = getElementsByTagName('li');
for(let i=0;i<aLi.length;i++){
  aLi[i].onclick = function(){
    setTimeout(()=>{
      console.log(this); //this -> aLi[i]
    },1000);
  }
}
```
输出结果为点击的dom元素，原因是箭头函数中的this指向父作用域，即沿当前作用域向上找，找到点击事件中当前的dom元素。
### 2.
```
let person1 = {
  name:'John',
  age:10,
  //此外部作用域没有this
  say:() => {
    console.log(this); //this -> window
  }
}
person1.say(); //window
```
箭头函数中
```
let person1 = {
  name:'John',
  age:10,
  say:function(){
    //外部作用域
    setTimeout(() => {
        console.log(this); //this指向外部作用域中的this
    });
  }
}
person1.say(); //输出对象person1
```
总结：箭头函数中的this，首先到它的父作用域找，如果父作用域还是箭头函数，那么接着向上找，直到找到this的指向。
[this原理](http://www.ruanyifeng.com/blog/2018/06/javascript-this.html)  
[this 指向详细解析（箭头函数）](https://www.cnblogs.com/dongcanliang/p/7054176.html)
