## Week 6

### DAY1：闭包和继承

#### 第一节：精讲


```
var scope = "global scope"; 
function checkScope() {
    var scope = "local scope";
    function f() {
        return scope;
    }
    return f();
}
checkScope(); ??
```


1. 什么是闭包
	
```
//全局作用域
function a() { 
//a的作用域
	var i = 0; //不能被销毁
	function b() { 
	//b的作用域
		alert(++i); 
	} 
	return b; 
} 
var c = a(); 
c();
```

> 当函数a的内部函数b被函数a外的一个变量引用的时候，就创建了一个闭包。（避免全局变量污染）

2. 闭包的特点
	* 函数嵌套函数
	* 内部函数可以使用外部函数的内部变量
	* 函数中的局部变量在外部是不能被引用（保护函数内的变量安全）
3. 为什么要写闭包
  * 避免全局变量污染
  * 将函数内部变量的值始终保存在内存中（在内存中维持一个变量）
  * 通过保护变量的安全实现JS私有属性和私有方法（不能被外部访问）

3. 全局变量污染
	
	```
		var a = 1
		function fn(){
			a++;
			console.log(a)
		}
		fn();
		fn();
		
		
		
		function fn(){
			var a = 1;
			a++;
			console.log(a)
		}
		
		fh()
		fn()
	```
	
3. 垃圾回收机制

	```
	function abc(){
		var str = 'vv'
	}
	abc() //使用完str被回收
	
	```
	
	
	* 参数和内部变量不会被回收
	
4. 闭包的应用场景

```有没有问题？
<!DOCTYPE html>
<html>
<head>
     <meta charset="UTF-8">
</head>
<body>
    <button>Button0</button>
    <button>Button1</button>
    <button>Button2</button>
    <button>Button3</button>
    <button>Button4</button>
</body>
</html>

<script>
var btns = document.getElementsByTagName('button');
for(var i = 0, len = btns.length; i < len; i++) {
    btns[i].onclick = function() {
        alert(i);
    }
}
</script>

```

```优化
for(var i = 0, len = btns.length; i < len; i++) {
    (function(i) {
        btns[i].onclick = function() {
            alert(i);
        }
    }(i))
}
```

```自执行函数 （函数声明  和 函数表达式）
	var counter = (function(){
        var privateCounter = 0; //私有变量
        function change(val){ //私有函数
            privateCounter += val;
        }
        return {
            increment:function(){   //三个闭包共享一个词法环境
                change(1);
            },
            decrement:function(){
                change(-1);
            },
            value:function(){
                return privateCounter;
            }
        };
    })();
```

5. 函数声明和函数表达式

#### 第二节：强化练习
1.   掌握闭包的特点和原理
2.   掌握闭包的应用场景

#### 第三节：精讲
1.   构造函数
	* 构造函数

	```普通函数
	var o1 = {
    	p: "I’m in Object literal",  
	   	alertP: function(){  
	        alert(this.p);  
	    }  
	}  
	```
	
	```普通对象
	function p(){
		var obj = new Object()
		obj.name = '123'
		obj.fn = function(){
			console.log(this.name)
		}
		return obj;
	}
	
	p.fn()
	```
	
	```构造函数
	
	function Co(){  
	    this.p = "I’m in constructed object";  
	    this.alertP = function(){  
	        alert(this.p);  
	    }  
	}  
	var o2 = new Co(); 
	```
	
	* 在函数内部对新对象（this）的属性进行设置，通常是添加属性和方法。
	* 为什么要使用构造函数?
		* 构造函数对一个新创建的对象进行初始化，增加一些成员变量和方法

2. prototype的概念 为什么使用原型

	```为什么使用原型
	
	function Co(){  
	    this.p = "I’m in constructed object";  
	    this.alertP = function(){  
	        alert(this.p);  
	    }  
	}  
	var o2 = new Co();
	var o3 = new Co();  
	
	o2.alertP == o3.alertP
	
	```

	```
	function C0(){  
		C0.prototype.p = "I’m in constructed object";  
	   	C0.prototype.alertP = function(){  
	        alert(this.p);  
	    }  
	}  
  var a = new C0();
  console.log(a)
	```
	
3. call/apply继承


  ```普通函数继承
  function fn(){
  	console.log(this.a)
  }

  fn.call({a:1})


   var a = {
        name : "fang" ,
        setName : (name) => {
            console.log(name)
        }
    }
    var b = {
      name : "zz"
    }
    a.setName.apply(b,[b.name]) 
  ```


  ```构造函数继承
  function C0(){  
      this.p = "I’m in constructed object";  
      this.alertP = function(){  
          alert(this.p);  
      }  
  }  
  function C1(){
     //继承了C0
    C0.call(this);
  }
  var o0 = new C0(); 
  var o1 = new C1(); 
  ```


  * call只能一个参数一个参数的传入。
  * apply则只支持传入一个数组，哪怕是一个参数也要是数组形式
  * this详解 

> 全局this指向 , 函数中this指向， 构造函数中this指向 ， 对象方法中call和apply 。
>
> 事件函数中的this   ， 箭头函数中的this。



4. 原型链继承

  ```
  function C1(){
    C1.prototype.name = 123;
    C1.prototype.sayName = function(){
      alert(this.name);
    };
  }

  function C2(){}

  C2.prototype = new C1();

  ```

5. ES6继承

    ```javascript
class Son extends Father{
    constructor(...arg){
        super(...arg)
    }
    play (){
   		super.play();
        // 更新内容
    }
    static sayHello(){
        
    }
    // 静态常量
    static get EVENT_ID(){
        return "EVENT_ID";
    }
}	
    ```



4. 混合继承

  ```javascript
  function C1(){
    this.name = 123;
    this.colors = ["red", "blue", "green"];
    this.sayB = function(){
      alert('123')
    }
  }

  C1.prototype.sayName = function(){
     alert(this.name);
  };

  console.log(abc);


  function C2(name, age){
  	//继承属性
    C1.call(this);
    this.age = age;
  }

  //C2.sayName => 没有

  C2.prototype = new C1(); //方法继承

  ```

#### 第四节：强化练习
1. 掌握继承的原理
2. 原型链
  * 实例对象和原型之间的链接
    * prototype 也是对象
    * __proto__
  * hasOwnProperty
  * constructor


  * Object.defineProperty

  ```
  function Abc(){
  }
  Abc.prototype.constructor = Abc
  ```

##### 拖拽小案例 

#### 第五节：强化练习

### DAY2：设计模式
#### 第一节：精讲
1. 单例模式
	* 如果每次创建新实例/功能/方法时，他的功能均完全相同，那么将其设置为单例。
2. 组合模式
3. 观察者模式

#### 第二节：强化练习

#### 第三节：精讲
1.	工厂模式
	.	策略模式
	.	代理模式
	.	适配器模式

#### 第四节：强化练习


#### 第五节：综合应用


### DAY3：购物车 

1.   cookie模拟登陆
2.   LocalStroage 模拟数据
3.   ES6编码
4.   模块化 => cookie登陆验证
		  => localStroage 数据

### DAY4：JQ

#### 第一节：精讲
1. JQ选择器
	* id 选择器
	* class 选择器
	* 元素选择器
2. JQ事件
	* 鼠标事件
	* 键盘事件
	* 表单事件
	* 文档/窗口事件
3. JQ效果
	* 隐藏和显示
	* 淡入淡出
	* 滑动
	* 动画

#### 第二节：强化练习


#### 第三节：精讲
1.   弹出层插件
2.   轮播图插件
3.   ajax插件

#### 第四节：强化练习
1.   树形目录

#### 第五节：强化练习
1. 掌握JQ的使用
2. 通过插件编写，理解JQ的封装原理

### DAY5：Jquery进阶

#### 第一节：精讲
1. JQ DOM
	* 获取内容和属性
	* 添加和删除元素
	* 获取并设置CSS
	* 尺寸方法
2. JQ 遍历
	* 祖先
	* 后代
	* 同胞(siblings)
	* 过滤
3. Ajax
	* AJAX get() 和 post() 方法

#### 第二节：应用
1. 图片轮播
2. 手风琴

#### 第三节：精讲
1. 图片翻转
2. 瀑布流
3. 九宫格拖拽

#### 第四节：应用

#### 第五节：综合应用
1. 掌握Jquery的常见API



