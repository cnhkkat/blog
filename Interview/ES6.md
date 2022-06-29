# ES6 的优化、升级 

## String

- 优化：`模板字符串`
- 升级：新增 `includes`(是否含有指定的字符串) 、 startsWith(是否以指定的字符串开始) 、endsWith() 、 padStart(从头填充)、padEnd()、 `repeat()`

## Array

- 优化：数组解构赋值、扩展运算符实现数组的复制
- 升级：新增 `find()`、includes(), fill()、flat()、copyWithin(从数组的指定位置拷贝元素到数组的另一个指定位置)

## Object
- 优化：
  - 对象解构赋值、扩展运算符可以取出对象的部分可遍历属性，实现对象的合并和分解。
  - 在 class 类里新增 `super` 关键字，super 指向当前函数所在对象的原型对象
- 升级： 
  - 新增 `Object.is()`方法，完善 === 严格等于的判断方法，原来 NaN 三个等于 NaN 判断为false , Object.is()修复了这个问题，判断为true。
  - 新增 `Object.assign()`方法，用于对象新增属性 或者 合并多个对象的自身属性
  - 新增 `getOwnPropertyDescriptors()` 方法，增强了 ES5 中 getOwnPropertyDescriptor()，用于获取指定对象的所有自身属性的描述对象。结合 `defineProperties()` 方法，可以复制对象，包括 get 、set 属性。
  - 新增了 `getPrototypeOf() 和 setPrototypeOf()` 方法，用来获取或设置当前对象的 prototype 对象。优化了 ES5 中 用 `__proto__`属性的问题
  - 新增了 `Object.keys()，Object.values()，Object.entries()`方法，用来获取对象的所有键、所有值和所有键值对数组

## Number

- 优化：新增 isFinite() 、`isNaN()`方法用来检测数值是否有限、是否是NaN。ES5中这两个方法判断非数值类型的参数是把它转换成number类型，这样会导致 字符串NaN 被判断为true，所以ES6优化了这个问题。
- 升级：在Math对象上新增了Math.cbrt()进行立方根的计算等

## Function

- 优化： 
  - 新增了`箭头函数`，箭头函数内的`this`指向的是函数定义时所在的对象，而不是函数执行时所在的对象。箭头函数内部没有自己的this，指向上一层的this，直到指向到有自己this的函数，把该this作为自己的this。箭头函数不能做构造函数，因为没有自己的this，不能实例化。
  - 函数的形参设置`默认值`
- 升级：
  - 新增了双冒号运算符，用来取代以往的bind，call,和apply（浏览器暂不支持，Babel已经支持转码)


# Symbol

  Symbol 是第七种数据类型，生成的值是独一无二的，可以避免对象属性变量名冲突覆盖的问题，不能被 for...in 遍历，但也不是私有属性

# Set、Map、WeakSet、WeakMap 的区别
- Set 类似数组，区别是成员是不重复的，可以`存储任何类型`的`唯一值`
- Map 类似对象，区别是 key 不只是是字符串，可以`遍历`