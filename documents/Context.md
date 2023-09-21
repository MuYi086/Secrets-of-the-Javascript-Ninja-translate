#### Context
1. 如果函数是一个对象的属性,会发生什么? 

    ```JS
    var katana = {
      isSharp: true,
      use: function () {
        this.isSharp = !this.isSharp
      }
    }
    katana.use()
    console.log(katana.isSharp, '校验isSharp的值是否改变')
    // false '校验isSharp的值是否改变'
    ```

1. 上下文表现是由什么确定的? 

    ```JS
    function katana () {
      this.isSharp = true
    }
    katana()
    console.log(isSharp === true, '全局对象现在存在这个名称和值')
    var shuriken = {
      toss: function () {
        this.isSharp = true
      }
    }
    shuriken.toss()
    console.log(shuriken.isSharp === true, '当它是对象的属性,这个值在对象之内设置')
    // true '全局对象现在存在这个名称和值'
    // true '当它是对象的属性,这个值在对象之内设置'
    ```

1. 我们怎么改变函数的上下文? 

    ```JS
    var object = {}
    function fn () {
      return this
    }
    console.log(fn() === this, '这个上下文是全局对象')
    console.log(fn.call(object) === object, '这个上下文被改变成一个特殊的对象')
    // true '这个上下文是全局对象'
    // true '这个上下文被改变成一个特殊的对象'
    ```

1. 不同的方式改变上下文 

    ```JS
    function add (a, b) {
      return a + b
    }
    console.log(add.call(this, 1, 2) === 3, 'call() 传入一个一个参数')
    console.log(add.apply(this, [1, 2]) === 3, 'apply() 传入一个数组作为参数')
    // true 'call() 传入一个一个参数'
    // true 'apply() 传入一个数组作为参数'
    ```

1. 我们如何在执行循环中使用回调? 

    ```JS
    function loop (array, fn) {
      for (var i = 0; i < array.length; i++) {
        fn.call(array, array[i], i)
      }
    }
    var num = 0
    loop([0, 1, 2], function (value, i) {
      console.log(value === num++, '确认上下文是我们期待的那样')
      console.log(this instanceof Array, '上下文应该是完整的数组')
    })
    // true '确认上下文是我们期待的那样'
    // true '上下文应该是完整的数组'
    // true '确认上下文是我们期待的那样'
    // true '上下文应该是完整的数组'
    // true '确认上下文是我们期待的那样'
    // true '上下文应该是完整的数组'
    ```