#### Instance Type

1. 审查对象基础

    ```JS
    // 三者之间的关系是:
    // 实例的constructor指向了构造函数本身
    // 实例的__proto__属性指向了构造函数的原型对象
    // 构造函数的原型指向了原型对象,原型对象的constructor指回了构造函数
    function Ninja () {}
    var ninja = new Ninja()
    console.log(typeof ninja === 'object', '然而用typeof判断实例始终是一个object')
    console.log(ninja instanceof Ninja, '对象被正确实例化')
    console.log(ninja.constructor === Ninja, 'ninja对象是由Ninja函数创建的')
    // true '然而用typeof判断实例始终是一个object'
    // true '对象被正确实例化'
    // true 'ninja对象是由Ninja函数创建的'
    ```

1. 我们可以使用 `constructor` 生成其他的实例

    ```JS
    function Ninja () {}
    var ninja = new Ninja()
    // 实例的constructor指向了构造函数本身
    var ninjaB = new ninja.constructor()
    console.log(ninjaB instanceof Ninja, '依然是一个ninja对象')
    // true '依然是一个ninja对象'
    ```

1. 在举一个 `Ninja` 的例子

    ```JS
    var ninja = (function () {
      function Ninja () {}
      return new Ninja()
    })()
    var ninjaB = new ninja.constructor()
    console.log(ninja.constructor === ninjaB.constructor, 'ninjas来源自同一个源')
    // true 'ninjas来源自同一个源'
    ```