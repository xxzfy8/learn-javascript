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