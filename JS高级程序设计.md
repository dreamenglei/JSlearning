# 函数内部属性 
## 在函数内部，有两个特殊的对象，arguments和this    
* __arguments__ ——储存传递给函数的每一个参数    
    
        function sayHi(name, message) {
        alert("Hello " + name + "," + message);
        } 
        这个函数可以进行重写，不显式地使用命名参数
        function sayHi() {
        alert("Hello " + arguments[0] + "," + arguments[1]);
        }   
    arguments主要用途是保存函数参数，但这个对象还有一个名叫 __callee的属性__    
    该属性是一个指针，指向拥有这个arguments对象的函数   

        function factorial(num){
            if (num <=1) {
                return 1;
            } else {
                return num * arguments.callee(num-1)
            }
        }   
        这是一个阶乘函数，函数体内没有引用函数名本身，无论引用函数时使用的是什么名字，都可以保证完成递归调用。
* __this__  ——函数据以执行的环境对象  

        window.color = "red"; 
        var o = { color: "blue" };  
        function sayColor(){  
        alert(this.color);  
        }  
        sayColor(); //"red"  
        o.sayColor = sayColor;  
        o.sayColor(); //"blue"   
函数本来是在全局作用域中定义的，引用了this对象，this是什么取决于在哪调用函数。  
将函数赋给对象o，并调用时，this引用的是对象o，故结果会是 blue。  
***
## 另一些常用函数对象的属性  
__caller__ ——调用当前函数的函数的引用（即谁调用了我） 

    function outer(){
    inner();
    }
    function inner(){
    alert(inner.caller);
    }
    outer();
以上代码会导致警告框中显示 outer()函数的源代码。因为 outer()调用了 inter()，所以
inner.caller 就指向 outer()，如果是在全局作用域中调用当前函数，它的值为null 
以上代码可改写为  

    function outer(){
        inner();
    }
    function inner(){
        alert(arguments.callee.caller);
    }
    outer();    
__length__ ——函数希望接收的命名参数的个数     
__prototype__ ——对于引用类型而言，prototype是保存它们所有实例方法的真正所在 

            function Person(){
            }
            Person.prototype.name = "Nicholas";
            Person.prototype.age = 29;
            Person.prototype.job = "Software Engineer";
            Person.prototype.sayName = function(){
                alert(this.name);
            };
            var person1 = new Person();
            person1.sayName(); //"Nicholas"
            var person2 = new Person();
        person2.sayName(); //"Nicholas"
        alert(person1.sayName == person2.sayName); //true   
我们将 sayName()方法和所有属性直接添加到了 Person 的 prototype 属性中，构造函数
变成了空函数。  
即使如此，也仍然可以通过调用构造函数来创建新对象，而且新对象还会具有相同的属
性和方法。  
但与构造函数模式不同的是，新对象的这些属性和方法是由所有实例共享的。换句话说，
person1 和 person2 访问的都是同一组属性和同一个 sayName()函数。
    
***   
## 每个函数都包含两个非继承而来的方法：apply()和call() 
两种方法的作用都是在特定的作用域中调用函数，实际上等于设置函数体内this对象的值 
* __apply()__   
apply()方法接收两个参数：一个
是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是 Array 的实例，也可以是
arguments 对象。    
* __call()__    
两种方法作用相同，call()传递给函数的参数必须逐个列举出来。  
* __事实上传递函数并非它们的用武之地，他们强大的地方是能够扩充函数运行的作用域__  

        window.color = "red";
        var o = { color: "blue" };
        function sayColor(){
            alert(this.color);
        }
        sayColor(); //red
        sayColor.call(this); //red
        sayColor.call(window); //red
        sayColor.call(o); //blue
    使用他们扩充作用域的最大好处，是对象不需要与方法有任何耦合关系。否则你需要先将saycolor函数放到对象o中，然后再通过o来调用他。    
* __bind()__  该方法会创建一个函数的实例，其this值会被绑定到传给bind()函数的值  

        window.color = "red";
        var o = { color: "blue" };
        function sayColor(){
            alert(this.color);
        }
        var objectSayColor = sayColor.bind(o);
        objectSayColor(); //blue    
    这里，saycolor()调用bind()并传入对象o，创建了objectColor函数，其this值等于o，因此即使是在全局作用域中调用这个函数，也会看到 blue




