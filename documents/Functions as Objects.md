#### Functions as Objects
1. 函数和对象有多相似?

    ```JS
    var obj = {}
    var fn = function () {}
    console.log(obj && fn, '对象和函数都存在')
    // function () {} '对象和函数都存在'
    ```

    ```JS
    var obj = {}
    var fn = function () {}
    obj.prop = 'some value'
    fn.prop = 'some value'
    console.log(obj.prop === fn.prop, '都是对象,都有属性')
    // true 都是对象,都有属性
    ```

1. 函数可以缓存返回值吗?

    ```JS
    // 原函数
    function getElements (name) {
      var results
      if (getElements.cache[name]) {
        results = getElements.cache[name]
      } else {
        results = documents.getElementsByTagName(name)
        getElements.cache[name] = results
      }
      return results
    }
    getElements.cache = {}
    console.log('Elements found: ', getElements('pre').length )
    console.log('Cache found: ', getElements.cache.pre.length)

    // 我们改造下，方便演示
    function square(a) {
      let result
      if (square.cache[a]) {
        result = square.cache[a]
      } else {
        result = a * a
        square.cache[a] = result
      }
      return result
    }
    square.cache = {}
    square(2)
    console.log(square.cache)
    // 4
    // {2: 4}
    ```

1. 一种合理的方式去缓存结果

    ```JS
    function isPrime (num) {
      if (isPrime.cache[num] !== null) {
        return isPrime.cache[num]
      }
      var prime = num !== 1
      for (var i = 2; i < num; i++) {
        if (num % i === 0) {
          prime = false
          break
        }
      }
      isPrime.cache[num] = prime
      return prime
    }
    isPrime.cache = {}
    console.log(isPrime(5), '确认函数工作了, 5是最好的')
    console.log(isPrime.cache[5], '确认结果已缓存')
    // true 确认函数工作了, 5是最好的
    // {5: true} 确认结果已缓存
    ```