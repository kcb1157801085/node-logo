 // this 指向问题： 一般情况下this指向它的调用者（构造函数会指向new出来的实例对象）（函数、对象只有在调用的时候才能确认this的指向，不调用不知道this的指向）

1、全局作用域或者普通函数中this指向全局对象window

2、自调用函数中的this  指向window

3、定时器（setTimeout ,setInterval）中的this  指向window

4、事件中的this 指向事件的调用者

5、 构造函数中this和原型对象中的this,都是指向构造函数new 出来实例对象

6、类 class中的this  指向由constructor构造器new出来的实例对象

7、箭头函数中的this （箭头函数自身没有this）它的this是看外层函数中的this指向的是那里，那么它就会跟外层函数的this指向相同，如果没有外层函数，这时候this就是指向的window

```javascript
         console.log(this);    //全局作用域下的this指向window

        function fn() {

           console.log(this);

​       }

​        // window.fn();   //函数调用时，才能确定this 的指向，这里的this指向的是window

​        // window.setTimeout(function() {

​        //     console.log(this);

​        // }, 1000);      //定时器里面的this  指向的是window
```

​        // 2. 方法调用中谁调用this指向谁

​       

```javascript
 var o = {

​            sayHi: function() {

​                console.log(this); // this指向的是 o 这个对象

​            }

​        }

​        o.sayHi(); // this === o    //方法调用时，this指向的是方法的调用者o

​        var fn = o.sayHi;  
注意此处的fn 是  o.sayHi 这个是函数，所以函数中的this指向的是window

​        fn() // window.fn()  this=== window  

​        console.log(1)

​        setTimeout(function(){

​            console.log(2)

​        })

​        setTimeout(function(){

​            console.log(3)

​        })

​        setTimeout(function(){

​            console.log(4)

​        })

​        console.log(5)

​        var button = document.querySelector('button');

​        button.onclick = function(){

​            console.log(6)

​        }

​        // var btn = document.querySelector('button');

​        // // btn.onclick = function() {

​        // //     console.log(this); // this指向的是btn这个按钮对象

        }

        btn.addEventListener('click', function() {

              console.log(this); // this指向的是btn这个按钮对象

          })
```

​        //     // 3. 构造函数中this和原型对象中的this,都是指向构造函数new 出来实例对象

​     

```javascript
   // function Fun() {

​        //     console.log(this); // this 指向的是fun 实例对象

​        // }

​        // var fun = new Fun();


```

改变this指向的几种方法

call()  方法的作用   1、可以调用函数 2、可以改变函数内部this的指向

主要作用可以实现父子构造函数  属性的继承

call(参数1，参数2，参数3 )    //参数一：设置成那个对象，那么this就会指向那个对象      参数2 .......  n  : **传递参数**

```javascript
 function Father(money,cars,house,company){
       this.money = money;
       this.cars = cars;
       this.house = house;
       this.company = company;
   }

 Father.prototype.management =  () => {
     console.log('我在管理一家公司');
 }

Son.prototype = new Father();     //方法的继承
Son.prototype.constructor = Son;  //原型对象的构造器一定要指回构造函数Son本身上

function Son(money,cars,house,company) {
    Father.call(this,money,cars,house,company);    //这里的this指向的是实例son
    
    
}


var sn = new Son(1000, '大众','海景别墅','阿里巴巴');
console.log(sn.money);
sn.management();
```

2、 apply (参数一，[ 参数2])；

参数一:要指向的那个对象，this就会指向这个对象      参数2：必须是数组

```javascript
var arr = [1,2,3,4,5];

var max = Math.max.apply(Math,arr);
var min = Math.min.apply(Math,arr);



```

3、bind ()  //bind() 方法不会调用函数,但是能改变函数内部this 指向,返回的是原函数改变this之后产生的新函数

```javascript
<button>点击</button>
    <script>
   var btn = document.querySelector('button');
   btn.onclick = function(){
       this.disabled = true;
       setTimeout(function(){
           this.disabled = false;
//此处在函数的外部调用bind方法，就会把原来定时器this指向window改成指向btn
       }.bind(this),3000)
   } 
    </script>
```



2.2.4 call、apply、bind三者的异同
共同点 : 都可以改变this指向;
不同点:
call 和 apply 会调用函数, 并且改变函数内部this指向.
call 和 apply传递的参数不一样,call传递参数使用逗号隔开,apply使用数组传递
bind 不会调用函数, 可以改变函数内部this指向.
应用场景

1. call 经常做继承.
2. apply经常跟数组有关系. 比如借助于数学对象实现数组最大值最小值
3. bind 不调用函数,但是还想改变this指向. 比如改变定时器内部的this指向