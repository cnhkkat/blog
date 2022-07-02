## 认识

> https://juejin.cn/book/7086408430491172901/section/7086435924271169550

1. TypeScript提供了强大的[类型检查能力](TypeScript/dev-env.md?id=类型检查)，通过`类型的流转`观察到变量的值是如何改变的，`类型的标记`也能帮助我们确保每一处访问、赋值与操作的类型是`符合预期`的。
2. TypeScript 限制了 JavaScript 的`灵活性`，但也增强了项目代码的`健壮性`，并且对于其他同属于灵活性的代表特性，如 this、原型链、闭包以及函数等，TypeScript 丝毫没有限制。
3. 由于 TypeScript 强大的`类型推导能力`，随着你对变量进行各种操作，TypeScript 就会自动地推导出变量最终的类型。敲击下`.`来访问一个变量的属性时，TypeScript 也会将所有的属性展示出来供你挑选，提高了开发效率。
4. 在最终编译时，TypeScript 会将这些类型代码抹除，返回可以放进浏览器里跑的、纯粹的 JavaScript 代码。因此，TypeScript 确实`不能提高应用程序的性能`，因为最终运行的仍然是 JavaScript。
5. 在小项目中，TypeScript 确实不可避免地降低了项目的开发效率。

- TypeScript 由三个部分组成：[类型](TypeScript/type.md)、[语法](TypeScript/syntax.md)、[工程](TypeScript/tsc.md)

代表了三个不同阶段使用 TypeScript 目的：为 JavaScript 代码添加类型与类型检查来确保`健壮性`，提前使用新语法或新特性来`简化代码`，以及最终获得`可用的 JavaScript 代码`。

- [类型基础](TypeScript/type-basic.md)
- [类型工具](TypeScript/type-tool.md)
- [类型系统](TypeScript/type-system.md)
- [类型编程](TypeScript/type-program.md)
- [demo练习](TypeScript/demo.md)



