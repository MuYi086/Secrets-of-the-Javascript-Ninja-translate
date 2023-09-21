#### Enforcing Function Context

1. 当我们给一个 `click` 绑定一个对象的方法是发生了什么? 

    ```JS
    var Button = {
      click: function () {
        this.clicked = true
      }
    }
    var elem = document.createElement('li')
    elem.innerHTML = '点击我'
    elem.onclick = Button.click
    document.querySelector('body').append(elem)
    elem.onclick()
    console.log(elem.clicked, '元素上意外的设置了clicked属性')
    ```

1. 我们需要将其上下文保持为原始对象

    ```JS
    function bind (context, name) {
      return function () {
        return context[name].apply(context, arguments)
      }
    }
    var Button = {
      click: function () {
        this.clicked = true
      }
    }
    var elem = document.createElement('li')
    elem.innerHTML = '点击我'
    elem.onclick = bind(Button, 'click')
    document.querySelector('body').append(elem)
    elem.onclick()
    console.log(Button.clicked, '元素上意外的设置了clicked属性')
    // true '元素上意外的设置了clicked属性'
    ```

1. 给所有函数添加一个允许强制上下文的方法

    ```JS
    Function.prototype.bind = function (object) {
      var fn = this
      return function () {
        return fn.apply(object, arguments)
      }
    }
    var Button = {
      click: function () {
        this.clicked = true
      }
    }
    var elem = document.createElement('li')
    elem.innerHTML = '点击我'
    // 利用bind改变当前执行函数的this指向
    elem.onclick = Button.click.bind(Button)
    document.querySelector('body').append(elem)
    elem.onclick()
    console.log(Button.clicked, 'clicked属性被正确的设置在对象上')
    // true 'clicked属性被正确设置到了对象上'
    ```

1. 我们终极目标(实现 `Prototype.js` 的 `bind` 方法)

    ```JS
    Function.prototype.bind = function () {
      // 执行函数
      var fn = this
      // 将arguments转成数组
      var args = Array.prototype.slice.call(arguments)
      // 取出第一参，也就是我们手动指定的this指向
      var object = args.shift()
      return function () {
        // 例如 aa.bind(bb)(c, d, e)
        // 因为bind是return一个方法,下文的arguments是该方法的参数,比如c,d,e
        return fn.apply(object, args.concat(Array.prototype.slice.call(arguments)))
      }
    }
    var Button = {
      click: function (value) {
        this.clicked = value
      }
    }
    var elem = document.createElement('li')
    elem.innerHTML = 'click me'
    elem.onclick = Button.click.bind(Button, false)
    document.querySelector('body').append(elem)
    elem.onclick()
    console.log(Button.clicked === false, 'clicked属性被正确设置到了对象上')
    // true 'clicked属性被正确设置到了对象上'
    ```