
# typescript

## 基本类型

### Boolean

`let isDone: boolean = false;`

### Number

`let decLiteral: number = 6;`

### String

`let name: string = 'bob'`

### Array

有两种方式可以定义数组。 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组。第二种方式是使用数组泛型，Array<元素类型>

`let list: number[] = [1, 2, 3];`

`let list: Array<number> = [1, 2, 3];`

### Object

object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。

### Tuple(元组)

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同

`let x: [string, number] = ['hello', 10]; `

### Enum(枚举)

enum类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值

```
enum Color {Red, Green, Blue}
let c: Color = Color.Green;

enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;

```

### Unknown 

我们可能需要描述编写应用程序时不知道的变量类型。这些值可能来自动态内容（例如，来自用户），或者我们可能希望有意接受API中的所有值。在这些情况下，我们希望提供一种类型，该类型告诉编译器和将来的读者该变量可以是任何变量，因此我们将其赋予unknown类型。

### Any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any类型来标记这些变量

```
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

与相比unknown，类型变量any允许您访问任意属性，即使不存在也是如此。这些属性包括函数，并且TypeScript不会检查它们的存在或类型

### Void

某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void

```
function warnUser(): void {
    console.log("This is my warning message");
}
```

### Null 和 Undefined

TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和null。 和 void相似，它们的本身的类型用处不是很大

默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。

### Never

never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

```
function error(message: string): never {
    throw new Error(message);
}
```

### Type assertions 类型断言

类似java的类型强转。有两种形式，其一是“尖括号”语法；另一个为as语法。

```
let someValue: unknown = "this is a string";
let strLength: number = (<string>someValue).length;

let someValue: unknown = "this is a string";
let strLength: number = (someValue as string).length;
```

## interfaces 接口

对值所具有的结构进行类型检查