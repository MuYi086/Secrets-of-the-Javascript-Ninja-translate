#### Inheritance

1. 原型继承工作原理的基础知识 

    ```JS
    function Person () {}
    Person.prototype.dance = function () {}
    function Ninja () {}
    Ninja.prototype = Person.prototype
    Ninja.prototype = { dance: Person.prototype.dance }
    console.log((new Ninja()) instanceof Person, '原型链不正确则会失败')
    Ninja.prototype = new Person()
    var ninja = new Ninja()
    console.log(ninja instanceof Ninja, 'ninja从Ninja的原型上接收到了功能')
    console.log(ninja instanceof Person, 'Person的原型')
    console.log(ninja instanceof Object, 'Object的原型')
    // false '原型链不正确则会失败'
    // true 'ninja从Ninja的原型上接收到了功能'
    // true 'Person的原型'
    // true 'Object的原型'
    ```

1. 让我们试试继承吧

    ```JS
    function Person () {}
    Person.prototype.getName = function () {
      return this.name
    }
    function Me () {
      this.name = 'MuYi086'
    }
    Me.prototype = new Person()
    var me = new Me()
    console.log(me.getName(), '设置了一个name')
    // MuYi086 设置了一个name
    ```