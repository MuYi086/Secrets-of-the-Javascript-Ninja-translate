#### Our Goal
1. 理解下面这个函数 
```JS
// The .bind method from Prototype.js
Function.prototype.bind = function () {
  // 以demo为例: aa.bind(c,d)(e,f)
  // 此this指向构造函数本身
  const fn = this
  // 利用数组的原型方法将arguments类数组结构转成数组,即[c,d]
  const args = Array.prototype.slice.call(arguments)
  // 取数组下标0，也就是入参的第一个参数,即c
  const object = args.shift()
  return function () {
    // 注意:这里的arguments是return出去的函数接受的参数,即[e,f]
    return fn.apply(object, args.concat(Array.prototype.slice.call(arguments)))
  }
}

// ES6写法
Function.prototype.bind = function (...argsA) {
  // 以demo为例: aa.bind(c,d)(e,f)
  const fn = this
  return function (...argsB) {
    // 注意:这里的arguments是return出去的函数接受的参数,即[e,f]
    return fn.apply(argsA.shift(), argsA.concat(argsB))
  }
}

// 举例
const aa = function () {
  console.log(this.name)
  console.log(arguments)
}
aa.bind({name: 'ougege'}, 1, 2)(3,4)
// ougege [1,2,3,4]
```