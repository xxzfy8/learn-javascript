### dom0与dom2
 >dom0事件绑定
 ```javascript
    ele.onclick = function() {
        // this.click
    }
 ```

 >dom2事件绑定
```javascript
    ele.addEventListener('click', function(e) {
        //this:ele
    }, false)
    // false: 让当前绑定的方法在冒泡阶段执行 （一般都是冒泡）
    // true: 在捕获阶段执行（一般不用)

```

### dom0与dom2的区别
>dom0
- 如果定义了两个dom0级事件，dom0级事件会覆盖
- 给当前元素对象的一个私有属性(onxxxx)赋值的过程(赋值前属性默认值为null, 赋值后相当于绑定了一个方法)
- 当赋值成功时(赋值一个函数)，此时浏览器会监听该dom元素的onxxxx，当用户触发会调用绑定的方法
- 移除时间 onxxxxx = null
- 多次赋值时后边的会覆盖前边的

>dom2
- addEventListener/attachEvent都是在EventTarget这个内置类的原型上定义的，我们调用使用的时候，首先通过原型链找到这个方法，然后执行完成事件绑定的效果
- 浏览器首先会给当前元素的某一个事件行为开辟一个事件池（事件队列）[其实是浏览器有一个统一的事件池，我们每个元素的某个行为绑定的方法都放在这个事件池中，只是通过相关的标识来区分的]，当我们通过addeventlistner做事件监听的时候，会把绑定的方法存放在事件池中
- 当元素的某一个行为触发，浏览器会到事件池中，把当前存放在事件池中的所有方法依次按照存放的先后顺序执行

>区别
- dom2支持dom0的所有事件 dom2独有的(DOMContentLoaded)
```javascript
window.onDOMContentLoaded  === undefined DOM0a中没有这个属性
window.addEventListener('DOMContentLoaded', function(){
    //标准浏览器中兼容这个事件：当浏览器中的DOM结构加载完成，就会出发这个事件（也会把绑定的方法执行）
}, false)
```
