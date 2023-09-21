#### Named Functions
1. 我们可以在函数的内部通过它的名称引用它自己 

    ```JS
    // 一个递归函数
    function yell (n) {
      return n > 0 ? yell(n - 1) + 'a' : 'hiy'
    }
    console.log(yell(4) == 'hiyaaaa')
    // true
    ```

1. 函数的名称是什么?

    ```JS
    // 把函数赋值给一个变量
    var ninja = function myNinja () {
      console.log(ninja === myNinja)
    }
    ninja()
    console.log(typeof myNinja === 'undefined')
    console.log(ninja)
    // true
    // true
    // function myNinja () {...}
    // 由此可见,函数赋值该变量后，仅可在函数内部引用，在函数外部是undefined
    ```

1. 如果匿名函数是对象的属性，那么我们可以执行它

    ```JS
    // 匿名函数赋值为对象的属性
    var ninja = {
      yell: function (n) {
        return n > 0 ? ninja.yell(n - 1) + 'a' : 'hiy'
      }
    }
    console.log(ninja.yell(4) === 'hiyaaaa')
    // true
    ```

1. 如果我们移除原始的对象,会发生什么?

    ```JS
    var ninja = {
      yell: function (n) {
        return n > 0 ? ninja.yell(n - 1) + 'a' : 'hiy'
      }
    }
    // 这里深究就是function是一个对象，存储在堆中,ninja.yell是一个引用地址
    var samurai = { yell: ninja.yell }
    // samurai 和 ninja 的属性yell 引用了同一个地址
    var ninja = null
    try {
      samurai.yell(4)
    } catch (e) {
      console.log(false, '报错了哦, ninja.yell 去哪了')
    }
    // false, 报错了哦, ninja.yell 去哪了
    // 由于yell函数调用了ninja.yell方法,导致抛错
    ```

1. 让我们给匿名函数一个名称

    ```JS
    var ninja = {
      yell: function yell (n) {
        // 注意这里: 递归的是它自己,而不是ninja.yell
        return n > 0 ? yell(n - 1) + 'a' : 'hiy'
      }
    }
    console.log(ninja.yell(4) === 'hiyaaaa')
    var samurai = { yell: ninja.yell }
    var ninja = {}
    console.log(samurai.yell(4) === 'hiyaaaa')
    // true
    // true
    ```

1. 如果我们不给函数命名呢?

    ```JS
    var ninja = {
      yell: function (n) {
        // arguments.callee 指向了它自己
        return n > 0 ? arguments.callee(n - 1) + 'a' : 'hiy'
      }
    }
    console.log(ninja.yell(4) === 'hiyaaaa')
    // true
    ```