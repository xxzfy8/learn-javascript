#new发生了什么
 -(1) 创建一个新对象;
 -(2) 将构造函数的作用域赋给新对象(因此 this 就指向了这个新对象);
 -(3) 执行构造函数中的代码(为这个新对象添加属性);
 -(4) 返回新对象。

```javascript
    // tset1
    function Test1(name) {
            this.name = name
        }
    Test1.prototype.aaa = function() {
        alert(this.name)
    }
    res1 = new Test1(111)

    // test2
    function Test2(name) {
        var obj = {}
        obj.name = name
        obj.__proto__ = Test.prototype
        return obj  
    }
    Test2.prototype.aaa = function() {
        alert(this.name)
    }
    res2 = Test(222)

```

    -   所有函数都自带一个属性是prototype（原型）它是一个对象类型
    -   原型中自带一个属性constructor（构造函数）指向期构造函数(类) 是当前函数自己(普通函数就是函数自己)
    -   __proto__当前所属类的对象原型(就是期构造函数的prototype)
    -           

```javascript
    function Person (name) {
      this.name = name
    }

    Person.prototype.fn = function () {
      console.log(this.name)
    }

    var p = new Person('小红')

    console.log(p.constructor === Person)   //true  实例的constructor指向期构造函数(类)
    console.log(p.__proto__ === Person.prototype)  //true 实例的__proto__指向期构造函数(类)的prototoye(原型) 
    console.log(p.__proto__.constructor === Person)  //true  构造函数(类)的原型中有constructor指向类自己
    console.log(p.__proto__.constructor === Person.prototype.constructor)  //true  

    function test1() {
        console.log(111)
    }
    console.log(test1.prototype.constructor === test1) // 普通函数的原型中的constructor指向函数自己
    console.log(test1.__proto__ === Function.prototype) // 普通函数的__proto__指向 函数类的prototype

```
