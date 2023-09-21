#### Flexible Arguments
1. 使用数量可变的参数对我们有利 

    ```JS
    function merge (root) {
      for (var i = 1; i < arguments.length; i++) {
        for (var key in arguments[i]) {
          root[key] = arguments[i][key]
        }
      }
      return root
    }
    var merged = merge({name: 'john'}, {city: 'boston'})
    console.log(merged.name === 'john', '原始name原封不动')
    console.log(merged.city === 'boston', 'city已经被复制了')
    // true '原始name原封不动'
    // true 'city已经被复制了'
    ```

1. 我们怎么在数组中找到最大最小值?

    ```JS
    function smallest (array) {
      // ES5
      return Math.min.apply(Math, array)
      // ES6
      return Math.min(...array)
    }
    function largest (array) {
      // ES5
      return Math.max.apply(Math, array)
      // ES6
      return Math.max(...array)
    }
    console.log(smallest([0, 1, 2, 3]) === 0, '定位最小值')
    console.log(largest([0, 1, 2, 3]) === 3, '定位最大值')
    // true '定位最小值'
    // true '定位最大值'
    ```

1. 另外一种可以的方式

    ```JS
    // 其实就是利用arguments对象
    function smallest () {
      return Math.min.apply(Math, arguments)
    }
    function largest () {
      return Math.max.apply(Math, arguments)
    }
    console.log(smallest(0, 1, 2, 3) === 0, '定位最小值')
    console.log(largest(0, 1, 2, 3) === 3, '定位最大值')
    // true '定位最小值'
    // true '定位最大值'
    ```

1. 为什么这里出错?

    ```JS
    function highest () {
      // 会报错,因为arguments对象是一个array-like对象,并不是一个array，没有array方法
      return arguments.sort(function (a, b) {
        return b - a
      })
    }
    console.log(highest(1, 1, 2, 3)[0] === 3, '拿到最高值')
    // arguments.sort is not a function
    ```

1. 我们必须将类 `array` 对象转成实际的 `array`

    ```JS
    function highest () {
      return makeArray(arguments).slice(1).sort(function (a, b) {
        return b - a
      })
    }
    function makeArray (array) {
      // 这里的Array()等同于[]
      return Array().slice.call(array)
    }
    console.log(highest(1, 1, 2, 3)[0] === 3, '拿到最高值')
    console.log(highest(3, 1, 2, 3, 4, 5)[1] === 4, '校验结果')
    // true '拿到最高值'
    // true '校验结果'
    ```

1. 我们可以使用 `call` 和 `apply` 去实现一个乘法函数

    ```JS
    function multiMax (multi) {
      // 此处multi默认代表第一参
      var allButFirst = Array().slice.call(arguments, 1)
      var largestAllButFirst = Math.max.apply(Math, allButFirst)
      return multi * largestAllButFirst
    }
    console.log(multiMax(3, 1, 2, 3) === 9, '3*3=9')
    // true '3*3=9'
    ```