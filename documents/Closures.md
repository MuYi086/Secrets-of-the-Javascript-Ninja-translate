#### Closures
1. 一个基础的闭包 

    ```JS
    var num = 10
    function addNum (myNum) {
      return num + myNum
    }
    console.log(addNum(5) === 15, '一起添加俩个数字, 一个来自于闭包')
    // true '一起添加俩个数字, 一个来自于闭包'
    ```

1. 那为什么这样不行呢?

    ```JS
    // 实际上是对window.num赋值
    var num = 10
    function addNum (myNum) {
      return num + myNum
    }
    num = 15
    console.log(addNum(5) === 15, '一起添加俩个数字, 一个来自于闭包')
    // false '一起添加俩个数字, 一个来自于闭包'
    ```
  
1. 闭包往往使用在回调、定时器 

    ```JS
    // 实际上在给window的属性赋值
    var count = 0
    var timer = setInterval (function () {
      if (count < 5) {
        console.log('timer call:', count)
        count++
      } else {
        console.log(count === 5, 'count来自于一个闭包,访问了每一个步骤')
        console.log(timer, 'timer的引用也是经过一个闭包')
        clearInterval(timer)
      }
    }, 100)
    // timer call: 0
    // timer call: 1
    // timer call: 2
    // timer call: 3
    // timer call: 4
    // true 'count来自于一个闭包,访问了每一个步骤'
    // timerId 'timer的引用也是经过一个闭包'
    ```

1. 闭包往往使用伴随着事件监听

    ```JS
    var count = 1
    var elem = document.createElement('li')
    elem.innerHTML = 'click me'
    elem.onclick = function () {
      console.log('Click #', count++)
    }
    document.querySelector('body').append(elem)
    console.log(elem.parentNode, '添加了可点击的元素')
    ```

1. 私有的属性,使用闭包

    ```JS
    // 这就是个简单的闭包,只有函数内部可以访问到
    function Ninja () {
      var slices = 0
      this.getSlices = function () {
        return slices
      }
      this.slice = function () {
        slices++
      }
    }
    var ninja = new Ninja()
    ninja.slice()
    console.log(ninja.getSlices() === 1, '我们可以访问内部切片数据')
    console.log(ninja.slices === undefined, '我们无法访问私有的数据')
    // true '我们可以访问内部切片数据'
    // true '我们无法访问私有的数据'
    ```

1. 最后一个相当狡猾，我们会重新访问它

    ```JS
    var a = 5
    function runMe (a) {
    // 由于变量提升和执行环境影响,a使用的入参,b使用的内部变量,c声明了未定义
      console.log(a === 6, '检查a的值')
      function innerRun () {
        console.log(b === 7, '检查b的值')
        console.log(c === undefined, '检查c的值')
      }
      var b = 7
      innerRun()
      var c = 8
    }
    runMe(6)
    // setTimeout是宏任务，会优先把for循环同步代码执行完在进入下一个宏任务处理
    for (var d = 0; d < 3; d++) {
      setTimeout(function () {
        console.log(d === 3, '检查d的值')
      }, 100)
    }
    // true '检查a的值'
    // true '检查b的值'
    // true '检查c的值'
    // true '检查d的值'
    // true '检查d的值'
    // true '检查d的值'
    ```