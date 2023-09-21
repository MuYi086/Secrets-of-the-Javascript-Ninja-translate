#### Built-in Prototypes

1. 我们可以修改内置对象原型 

    ```JS
    if (!Array.prototype.forEach) {
      console.log(2222)
      Array.prototype.forEach = function (fn) {
        // this指向了array本身
        for (var i = 0; i < this.length; i++) {
          fn(this[i], i, this)
        }
      }
    }
    ['a', 'b', 'c'].forEach(function (value, index, array) {
      console.log(value, '在的位置' + index + '从里面出来' + (array.length - 1))
    })
    // a 在的位置0从里面出来2
    // b 在的位置1从里面出来2
    // c 在的位置2从里面出来2
    ```

1. 当心:扩展原型是危险的 

    ```JS
    Object.prototype.keys = function () {
      var keys = []
      //  for in 会遍历原型属性
      for (var i in this) {
        keys.push(i)
      }
      return keys
    }
    var obj = { a: 1, b: 2, c: 3 }
    console.log(obj.keys().length === 3, '我们只有3个属性')
    delete Object.prototype.keys
    // false '我们只有3个属性'
    ```