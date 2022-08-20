#### Defining Functions
1. 什么方式取定义函数?

    ```JS
    // 具名函数
    function isNimble() {
      return true
    }
    // 赋值给变量
    var canFly = function () {
      return true
    }
    // 对象的属性
    window.isDeadly = function () {
      return true
    }
    ```

1. 函数定义的顺序重要吗?

    ```JS
    // 由于函数声明会存在变量提升，所以会优先
    var canFly = function () {
      return true
    }
    window.isDeadly = function () {
      return true
    }
    console.log(isNimble(), canFly(), isDeadly())
    function isNimble () {
      return true
    }
    // true true true
    ```

1. 赋值可以在哪里访问到?

    ```JS
    // 由于函数声明会存在变量提升
    console.log(typeof canFly == 'undefined')
    console.log(typeof isDeadly == 'undefind')
    var canFly = function () {
      return true
    }
    window.isDeadly = function () {
      return true
    }
    // false
    // false
    ```

1. 函数可以被定义在返回值语句后吗？

    ```JS
    // 由于函数声明会存在变量提升
    function stealthCheck () {
      console.log(stealth())
      return stealth()
      function stealth () {
        return true
      }
    }
    stealthCheck()
    // true
    // true
    ```