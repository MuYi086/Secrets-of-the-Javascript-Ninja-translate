#### Function Prototypes

1. 给函数增加一个原型方法 

    ```JS
    function Ninja () {}
    Ninja.prototype.swingSword = function () {
      return true
    }
    //  此处相当于执行了这个函数,空函数无返回值
    var ninjaA = Ninja()
    console.log(!ninjaA, '是undefined, 并不是Ninja的实例')
    // 使用new关键字才是new了一个实例
    var ninjaB = new Ninja()
    console.log(ninjaB.swingSword(), '方法存在并且调用了')
    // true '是undefined, 并不是Ninja的实例'
    // true '方法存在并且调用了'
    ```

1. 属性添加到构造函数上覆盖了原型属性

    ```JS
    // 考察的是原型链查找顺序，优先实例自身属性和方法开始查找,然后才是原型链
    function Ninja () {
      this.swingSword = function () {
        return true
      }
    }
    Ninja.prototype.swingSword = function () {
      return false
    }
    var ninja = new Ninja()
    console.log(ninja.swingSword(), '调用实例的方法,不是原型的方法')
    // true '调用实例的方法,不是原型的方法'
    ```

1. 原型属性同时影响同一构造函数的所有对象,即使他们已经存在

    ```JS
    // 这里考察的是实例与构造函数原型对象关系
    // 其实是实例的__proto__属性指向了构造函数的原型对象
    function Ninja () {
      this.swung = true
    }
    var ninjaA = new Ninja()
    var ninjaB = new Ninja()
    Ninja.prototype.swingSword = function () {
      return this.swung
    }
    console.log(ninjaA.swingSword(), '方法存在,即使是无序的')
    console.log(ninjaB.swingSword(), '也包括所有实例话的对象上')
    // true '方法存在,即使是无序的'
    // true '也包括所有实例话的对象上'
    ```

1. 给函数添加一个原型以达到 `return` 它自己并且修改自有属性

    ```JS
    function Ninja () {
      this.swung = true
    }
    var ninjaA = new Ninja()
    var ninjaB = new Ninja()
    // 其实就是利用的原型对象上函数和构造函数公用一个this
    Ninja.prototype.swing = function () {
      this.swung = false
      return this
    }
    console.log(!ninjaA.swing().swung, '检查swing方法存在并且return了一个实例')
    console.log(!ninjaB.swing().swung, '它同时在所有Ninja实例上起作用')
    // true '检查swing方法存在并且return了一个实例'
    // true '它同时在所有Ninja实例上起作用'
    ```