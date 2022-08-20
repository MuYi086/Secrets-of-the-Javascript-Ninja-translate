#### Temporary Scope

1. 自执行,临时，函数

    ```JS
    (function () {
      var count = 0
      var timer = setInterval(function () {
        if (count < 5) {
          console.log('Timer call: ', count)
          count++
        } else {
          console.log(count === 5, 'Count来自于一个闭包,每一个步骤')
          console.log(timer, 'timer的引用也是来自于一个闭包')
          clearInterval(timer)
        }
      }, 100)
    })()
    console.log(typeof count === 'undefined', 'count在函数之外不存在')
    console.log(typeof timer === 'undefined', 'timer也是')
    // true 'count在函数之外不存在'
    // true 'timer也是'
    // Timer call:  0
    // Timer call:  1
    // Timer call:  2
    // Timer call:  3
    // Timer call:  4
    // true 'Count来自于一个闭包,每一个步骤'
    // 1 'timer的引用也是来自于一个闭包'
    ```

1. 现在我们可以操作闭包和循环

    ```JS
    // 把d作为参数传入自执行函数
    for (var d = 0; d < 3; d++) (function (d) {
      setTimeout(function () {
        console.log('d的值: ', d)
        console.log(d === d, '检查d的值')
      }, d * 200)
    })(d)
    // d的值: 0
    // true '检查d的值'
    // d的值: 1
    // true '检查d的值'
    // d的值: 2
    // true '检查d的值'
    ```

1. 修复这个循环中错误的闭包

    ```JS
    // 方法一: for循环中var改成let
    var count = 0
    for (let i = 0; i < 4; i++) {
      setTimeout(function () {
        console.log(i === count++, '检查i的值')
      }, i * 200)
    }

    // 方法二: 匿名函数传参
    var count = 0
    for (var i = 0; i < 4; i++) (function (d) {
      setTimeout(function () {
        console.log(d === count++, '检查i的值')
      }, i * 200)
    })(i) 
    ```
