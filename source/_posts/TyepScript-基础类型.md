---
title: TypeScript-基础类型
date: 2020-05-20 11:21:18
tags: [typeScript]
categories: typeScript

---

TypeScript 支持与 JavaScript 几乎相同的数据类型，此外还提供了一些其他类型。

## 布尔值 boolean

最基本的数据类型之一，值为`true`或`false`

```typescript
let isOK: boolean = false;
```

## 字符串 string

最基本的数据类型之一，可以使用双引号（""）或者单引号（''）表示字符串。

```typescript
let str: string = "hello";
let strWorld: string = 'world';
```

还可以使用模板字符串定义多行或嵌入表达式，使用反引号（\`）定义字符串，`${expr}`表示嵌入表达式。

```typescript
let name: string = "小明";
let sentence: strinbg = `你好，${name} !`;
```

## undefined 和 null

`undefined`和`null`两者有各自的类型分别叫做`undefined`和`null`。

```typescript
let u: undefined = undefined;
let n: null = null;
```

默认情况下`undefined`和`null`是所有类型的子类型，也就是说可以把`undefined`和`null`赋值给任意类型的变量。

如果开启 `--strictNullChecks` 标记，`undefined`和`null`只能赋值给`void`和它们本身，这时也许在某处你想传入一个 `string`或`null`或`undefined`，你可以使用联合类型`string | null | undefined`。

## 数组 array

TypeScript 有两种方式定义数组。

一种在元素类型后面加`[]`

```typescript
let arrayList: number[] = [1, 2, 3];
```

另一种使用数组泛型`Array<元素类型>`

```typescript
let arrayList: Array<number> = [1, 2, 3];
```

## 元组 tuple

元组类型允许标识一个已知元素数量和类型的数组，各个元素的类型可以不同。

```typescript
let tupleList: [string, number];
tupleList = ["hello", 10];
```

访问一个越界元素，会使用联合类型替代

```typescript
tupleList[2] = "world"; // ok , 字符串可以赋值给(string | number)类型
tupleList[2] = true; // Error , 布尔不是(string | number)类型
```

## 枚举 enum

`enum`是对 JavaScript 标准数据类型的一个补充

```typescript
enum Color {Red, Green, Blue};
let c: Color = Color.Green;
```

或者，全部都采用手动赋值

```typescript
enum Color {Red = 1, Green = 2, Blue = 4};
let c: Color = Color.Green;
```

枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字

```typescript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```

## 对象 object

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

使用`object`类型，就可以更好的表示像`Object.create`这样的API。

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

## 任何类型 any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

在对现有代码进行改写的时候，`any`类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。 你可能认为 `Object`有相似的作用，就像它在其它语言中那样。 但是 `Object`类型的变量只是允许你给它赋任意值，但是却不能够在它上面调用任意的方法，即便它真的有这些方法

```typescript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists 可能在运行时存在
notSure.toFixed(); // okay, toFixed 存在但编译器不进行检查

let prettySure: Object = 4;
prettySure.toFixed(); // Error: 属性 'toFixed' 在 'Object' 上不存在
```

当你只知道一部分数据的类型时，`any`类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据

```typescript
let list: any[] = [1, true, "free"];
list[1] = 100;
```

## 无类型 void

某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`

```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`

```typescript
let unusable: void = undefined;
```

## 不存在的类型 never

`never`类型表示的是那些永不存在的值的类型。 例如， `never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 `never`类型，当它们被永不为真的类型保护所约束时。

`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，*没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使 `any`也不可以赋值给`never`。

下面是一些返回`never`类型的函数

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

## 类型断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一个为`as`语法

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，**当你在 TypeScript 里使用 JSX时，只有 `as`语法断言是被允许的**。

## 