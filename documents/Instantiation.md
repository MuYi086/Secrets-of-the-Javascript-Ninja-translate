#### Instantiation
1. `new` 操作符做了什么?

    ```JS
    function Ninja () {
      this.name = 'Ninja'
    }
    var ninjaA = Ninja()
    // 由于Ninja函数没有return, 所以ninjaA是undefined
    console.log(!ninjaA, '是undefined, 不是Ninja的实例')
    var ninjaB = new Ninja()
    console.log(ninjaB.name === 'Ninja', '属性存在于ninja的实例上')
    // true '是undefined, 不是Ninja的实例'
    // true '属性存在于ninja的实例上'
    ```

1. 我们 `this` 的上下文是一个 `Ninja` 对象

    ```JS
    function Ninja () {
      this.swung = false
      this.swingSword = function () {
        this.swung = !this.swung
        return this.swung
      }
    }
    var ninja = new Ninja()
    console.log(ninja.swingSword(), '调用实例上的方法')
    console.log(ninja.swung, 'ninja 挥动了剑')
    var ninjaB = new Ninja()
    console.log(!ninjaB.swung, '确认ninja 没有挥动剑')
    // true, '调用实例上的方法'
    // true, 'ninja 挥动了剑'
    // true '确认ninja 没有挥动剑'
    ```

1. 给对象添加一个新属性和方法

    ```JS
    function Ninja (name) {
      this.changeName = function (name) {
        this.name = name
      }
      this.changeName(name)
    }
    var ninja = new Ninja('john')
    console.log(ninja.name === 'john', '名称在初始化已经被设置')
    ninja.changeName('bob')
    console.log(ninja.name === 'bob', '名称已经成功改变')
    // true '名称在初始化已经被设置'
    // true '名称已经成功改变'
    ```

1. 当我们忘记使用 `new` 操作符会发生什么?

    ```JS
    // 方式一
    function User (first, last) {
      this.name = first + ' ' + last
    }
    var user = User('john', 'resig')
    console.log(typeof user === 'undefined', '不使用new, 实例就是undefined')
    // true '不使用new, 实例就是undefined'


    // 方式二
    function User (first, last) {
      this.name = first + ' ' + last
    }
    window.name = 'resig'
    // 这里相当于执行函数,函数中的this指向window
    var user = User('john', name)
    console.log(name === 'johnresig', 'name变量被偶然的覆盖了')
    // true 'name变量被偶然的覆盖了'
    ```

1. 我们要确认 `new` 操作符总是被使用的

    ```JS
    function User (first, last) {
      if (!(this instanceof User)) {
        return new User(first, last)
      }
      this.name = first + '' + last 
    }
    var name = 'resig'
    var user = User('john', name)
    console.log(user, '即使它是错误的，也被正确定义了')
    console.log(name === 'resig', '正确的name是被保持的')
    // {name: 'johnresig'} '即使它是错误的，也被正确定义了'
    // true '正确的name是被保持的'
    ```

1. 有没有更像类的方式完成呢?

    ```JS
    // arguments.callee指向了它本身
    function User (first, last) {
      if (!(this instanceof arguments.callee)) {
        return new User(first, last)
      }
      this.name = first + '' + last
    }
    var name = 'resig'
    var user = User('john', name)
    console.log(user, '即使它是错误的，也被正确定义了')
    console.log(name === 'resig', '正确的name是被保持的')
    // {name: 'johnresig'} '即使它是错误的，也被正确定义了'
    // true '正确的name是被保持的'
    ```