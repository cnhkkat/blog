## null undefined
JS 中的7种原始类型在 TS 中都有对应的类型
```js
const name: string = 'linbudu';

const undef: undefined = undefined;
const nul: null = null;
```

除了 null 和 undefined 


- 在 JS 中表示 **不存在的对象**(原型链的终点、函数参数) 和 **未定义值** (用在变量、参数、属性、函数返回值)
- 在 TS 中都是**有具体意义的类型**，在关闭 `strictNullChecks` 检查时会被看做是 **其他类型的子类型** 

- 比如 string 类型会被认为包含了 null 与 undefined 类型。

```js
// 在关闭 strictNullChecks 时成立
const tmp3: string = null;
const tmp4: string = undefined;
```

TS 存在一个特殊的类型 void

## void

- JS 中的 void 是**执行后面的表达式并返回 undefined**
- TS 中的 void 是**描述一个内部没有 return 的语句**

```js
const voidVar1: void = undefined;
// 关闭 strictNullChecks
const voidVar2: void = null;
```

## 数组

TS 中有两种方式声明数组类型
```js
const arr1: string[] = [];

const arr2: Array<string> = [];
```

使用 元组（Tuple）代替 数组，在**越界访问**时给出类型报错。

## 元组

```js
const arr4: [string, string, string] = ['lin', 'bu', 'du'];
// 长度为“3”的元组类型“[string, string, string]”在索引“599“处没有元素
console.log(arr4[599]); 

// 长度为 "3" 的元组类型 "[string, number, boolean]" 在索引 "3" 处没有元素。
const [name, age, male, other] = arr4;


const arr5: [string, number, boolean] = ['linbudu', 599, true];

// 标记为可选的成员，在 --strictNullCheckes 配置下会被视为一个 string | undefined 的类型。
const arr6: [string, number?, boolean?] = ['linbudu'];
// 1 | 2 | 3 这个元组的长度可能为 1、2、3
type TupleLength = typeof arr6.length; 
```

## 具名元组
```js
const arr7: [name: string, age: number, male: boolean] = ['linbudu', 599, true];

const arr7: [name: string, age: number, male?: boolean] = ['linbudu', 599, true];
```


## 对象

使用 `interface` 声明一个结构，使用这个结构作为一个对象的**类型标注** 来描述对象类型。
```js
interface IDescription {
  name: string;
  age: number;
  male: boolean;
}

const obj1: IDescription = {
  name: 'linbudu',
  age: 599,
  male: true,
};
```
- 每个属性值一一对应接口的属性类型
- 属性个数对应，不能 通过 . 增加属性

**修饰接口属性**

- optional 可选 ? 

```js
interface IDescription {
  name: string;
  age: number;
  male?: boolean;
  func?: Function;
}

const obj2: IDescription = {
  name: 'linbudu',
  age: 599,
  male: true, 
  // 无需实现 func 也是合法的
};

obj2.male = false; // 访问时 类型是 boolean | undefined 

obj2.func(); // 不能调用可选方法
obj2.func = () => {}; // 但可以赋值。类型以接口的描述为准

```

- 只读 readonly 。防止对象的属性再次赋值。

```js
interface IDescription {
  readonly name: string;
  age: number;
}

const obj3: IDescription = {
  name: 'linbudu',
  age: 599,
};

// 无法分配到 "name" ，因为它是只读属性
obj3.name = "林不渡";
```

在数组与元组层面也有着只读的修饰，但与对象类型有着两处不同。

- 你只能将整个数组/元组标记为只读，而不能像对象那样标记某个属性为只读。

- 一旦被标记为只读，那这个只读数组/元组的类型上，将不再具有 push、pop 等方法（即会修改原数组的方法），因此报错信息也将是类型 xxx 上不存在属性“push”这种。这一实现的本质是只读数组与只读元组的类型实际上变成了 ReadonlyArray，而不再是 Array。


## type

- type 类型别名 用来将一个函数签名、一组联合类型、一个工具类型等抽离成一个完整独立的类型
- interface 用来描述对象、类的结构
- 大部分场景下 type 可以代替 interface 描述对象

## Object、object、{}

- JS 中原型链的顶端是 Object 
- TS 中 Object **包含了所有类型**  。 对于 undefined、null、void 0 需要关闭 strictNullChecks。



```js
const tmp1: Object = undefined;
const tmp2: Object = null;
const tmp3: Object = void 0;

const tmp4: Object = 'linbudu';
const tmp5: Object = 599;

const tmp6: Object = { name: 'linbudu' };
const tmp7: Object = () => {};
const tmp8: Object = [];
```
**object** 的引入就是为了解决对 Object 类型的错误使用，它代表**数组、对象与函数类型**

```js
const tmp17: object = undefined;
const tmp18: object = null;
const tmp19: object = void 0;

const tmp22: object = { name: 'linbudu' };
const tmp23: object = () => {};
const tmp24: object = [];
```

**{}** 作为类型签名就是一个合法的，但**内部无属性定义**的**空对象**，这类似于 Object（想想 new Object()），它意味着任何**非 null / undefined 的值**

```js
const tmp28: {} = 'linbudu';
const tmp29: {} = 599;
const tmp30: {} = { name: 'linbudu' };
const tmp31: {} = () => {};
const tmp32: {} = [];

const tmp30: {} = { name: 'linbudu' };
// X 类型“{}”上不存在属性“age”。
tmp30.age = 18;
```

- 无法对这个变量进行任何赋值操作,因为 {} 什么属性都没有。

📢 注意：
- 不要使用Object以及类似的装箱类型(String等)
- 当你不确定某个变量的具体类型，但能确定它不是原始类型，可以使用 object。最好进一步区分。
  - `Record<string, unknown> `或 `Record<string, any> `表示对象，
  - `unknown[]` 或 `any[]` 表示数组,
  - `(...args: any[]) => any`表示函数。

- 避免使用{}。{}意味着任何非 null / undefined 的值，从这个层面上看，使用它和使用 any 一样恶劣