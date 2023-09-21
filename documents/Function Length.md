#### Function Length

1. 函数的 `length` 属性是如何工作的? 

    ```JS
    function makeNinja (name) {}
    function makeSamurai (name, rank) {}
    console.log(makeNinja.length === 1, '只需要一个参数')
    console.log(makeSamurai.length === 2, '需要多个参数')
    // true '只需要一个参数'
    // true '需要多个参数'
    ```

1. 我们可以使用它实现方法重载

    ```JS
    function addMethod (object, name, fn) {
      var old = object[name]
      object[name] = function () {
        // 此处的arguments代指实例执行find方法传入的参数
        if (fn.length === arguments.length) {
          return fn.apply(this, arguments)
        } else if (typeof old === 'function') {
          return old.apply(this, arguments)
        }
      }
    }
    function Ninjas () {
      var ninjas = ['张三', '李四', '王五']
      addMethod(this, 'find', function () {
        return ninjas
      })
      addMethod(this, 'find', function (name) {
        var ret = []
        for (var i = 0; i < ninjas.length; i++) {
          if (ninjas[i].indexOf(name) === 0) {
            ret.push(ninjas[i])
          }
        }
        return ret
      })
      addMethod(this, 'find', function (first, last) {
        var ret = []
        for (var i = 0; i < ninjas.length; i++) {
          var ret = []
          if (ninjas[i] === (first + '' + last)) {
            ret.push(ninjas[i])
          }
        }
        return ret
      })
    }
    var ninjas = new Ninjas()
    // 重点在于判断当前执行的find方法的length和fn的length
    console.log(ninjas.find().length === 3, '找到所有ninjas')
    console.log(ninjas.find('李').length === 1, '通过姓氏找到ninajs')
    console.log(ninjas.find('张', '三').length === 1, '通过姓和名找到ninjas')
    console.log(ninjas.find('王', 'X', '五') === null, '不执行')
    // true '找到所有ninjas'
    // true '通过姓氏找到ninajs'
    // false '通过姓和名找到ninjas'
    // false '不执行'
    ```