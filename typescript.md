#      TS
* 资料来源**尚硅谷**：
  * [尚硅谷vue3+TS](https://24kcs.github.io/vue3_study)
  ************************************
## TypeScript快速上手
*  全局安装 TypeScript：
```powershell
npm install -g typescript
```
* 安装完成后，在控制台运行如下命令，检查安装是否成功(3.x):本人安装4.x
```powershell
tsc -V 
```
### 编写 TS 程序
* [ts_helloWorld](./tsDemo/helloworld.ts)
* 手动编译代码
> 我们使用了 .ts 扩展名，但是这段代码仅仅是 JavaScript 而已。
* 在命令行上，运行 TypeScript 编译器：
```powershell
tsc helloworld.ts
```
* 输出结果为一个 [helloworld.js](./tsDemo/helloworld.js) 文件，它包含了和输入文件中相同的 JavsScript 代码。
* 在命令行上，通过 Node.js 运行这段代码：
```powershell
node helloworld.js
```
* vscode自动编译
  1. 在命令行上`tsc --init`,生成配置文件[tsconfig.json](./tsDemo/tsconfig.json)

  2. 修改tsconfig.json配置
      - "outDir": "./js",
      - "strict": false,    
  3. 启动监视任务: 
    - 终端 -> 运行任务 -> 监视tsconfig.json
### 类型注解
* 接下来让我们看看 TypeScript 工具带来的高级功能。 给函数的 person 参数添加 : string 类型注解，如下：
  
```typescript
function greeter (person: string) {
return 'Hello, ' + person
}

let user = 'Yee'

console.log(greeter(user))
```
> TypeScript 里的类型注解是一种轻量级的为函数或变量添加约束的方式。 在这个例子里，我们希望 greeter 函数接收一个字符串参数。 然后尝试把 greeter 的调用改成传入一个数组：
```typescript
function greeter (person: string) {
return 'Hello, ' + person
}
let user = [0, 1, 2]
console.log(greeter(user))
```
* 重新编译，你会看到产生了一个错误：
```typescript
error TS2345: Argument of type 'number[]' is not  assignable to parameter of type 'string'.
```
* 类似地，尝试删除 greeter 调用的所有参数。 TypeScript 会告诉你使用了非期望个数的参数调用了这个函数。 在这两种情况中，TypeScript提供了静态的代码分析，它可以分析代码结构和提供的类型注解。

> 要注意的是尽管有错误，greeter.js 文件还是被创建了。 就算你的代码里有错误，你仍然可以使用 TypeScript。但在这种情况下,TypeScript 会警告你代码可能不会按预期执行。
### 接口 interface
* 让我们继续扩展这个示例应用。这里我们使用接口来描述一个拥有 firstName 和 lastName 字段的对象。 在 TypeScript 里，只在两个类型内部的结构兼容，那么这两个类型就是兼容的。 这就允许我们在实现接口时候只要保证包含了接口要求的结构就可以，而不必明确地使用 implements 语句。
```typescript
interface Person {
  firstName: string
  lastName: string
}

function greeter (person: Person) {
  return 'Hello, ' + person.firstName + ' ' + person.lastName
}

let user = {
  firstName: 'Yee',
  lastName: 'Huang'
}

console.log(greeter(user))
```
> 函数的形参是一个拥有两个必填属性的接口，实参中必须传递两个属性名相同的字段，且类型保持一致

### 类

* TypeScript 支持 JavaScript 的新特性，比如支持基于类的面向对象编程。

* 让我们创建一个 User 类，它带有一个构造函数和一些公共字段。因为类的字段包含了接口所需要的字段，所以他们能很好的兼容。

* 还要注意的是，我在类的声明上会注明所有的成员变量，这样比较一目了然。
```typescript
class User {
  fullName: string
  firstName: string
  lastName: string

  constructor (firstName: string, lastName: string) {
    this.firstName = firstName
    this.lastName = lastName
    this.fullName = firstName + ' ' + lastName
  }
}

interface Person {
  firstName: string
  lastName: string
}

function greeter (person: Person) {
  return 'Hello, ' + person.firstName + ' ' + person.lastName
}

let user = new User('Yee', 'Huang')

console.log(greeter(user))
```
* 重新运行` tsc user_class.ts`，你会看到 TypeScript 里的类只是一个语法糖，本质上还是 JavaScript 函数的实现

* [user_class.ts](./tsDemo/user_class.ts)与[user_class.js](./tsDemo/user_class.js)

  > 函数类型受接口的约束



### 使用webpack打包TS
#### 下载依赖

* `html-webpack-plugin` 对html打包
* `clean-webpack-plugin`清除之前打包好的js文件
* `cross-env`跨平台命令
* 当前使用的版本：
  * webpack@4.41.5
  * webpack-cli@3.3.10
  * webpack-dev-server@3.10.2

```powershell
 yarn add -D typescript
 yarn add -D webpack webpack-cli
 yarn add -D webpack-dev-server
 yarn add -D html-webpack-plugin clean-webpack-plugin
 yarn add -D ts-loader
 yarn add -D cross-env
```
******************************************************************
```powershell
npm install -D typescript
npm install -D webpack webpack-cli
npm install -D webpack-dev-server
npm install -D html-webpack-plugin clean-webpack-plugin
npm install -D ts-loader
npm install -D cross-env
```
#### 入口JS: src/main.ts
* `document.write('Hello Webpack TS!')`





#### index页面: public/index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>webpack & TS</title>
</head>
<body>
    <!--最好将所有的ts代码编译为ts代码再引入-->
    <!--ts文件中有ts语法代码-->
    <script src="xxx.js"></script>
     <!--ts文件中没有ts语法代码-->
    <script src="xxx.ts"></script>
         
</body>
</html>
```
> 总结：
>
> ts的文件中如果直接书写js语法的代码，那么html文件中直接引入ts文件，在谷歌浏览器中是可以使用的。
>
> 如果ts文件中有了ts语法代码，那么就需要把这个ts文件编译成js文件，在html文件中引入js文件来使用。
>
> ts文件中函数的形参，如果使用了某个类型进行修饰，那么最终在编译的js文件中是没有这个类型的。
>
> ts文件中的变量使用let进行修饰，编译的js文件中的修饰符就变成var了



#### build/webpack.config.js

```json
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

const isProd = process.env.NODE_ENV === 'production' // 是否生产环境

function resolve (dir) {
  return path.resolve(__dirname, '..', dir)
}

module.exports = {
  mode: isProd ? 'production' : 'development',//模式
  entry: {
    app: './src/main.ts'//入口文件
  },

  output: {
    path: resolve('dist'),//打包好的文件存放位置
    filename: '[name].[contenthash:8].js'//打包好的文件名
  },

  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',//使用ts-loader这个包对ts一级ts文件进行处理，处理src目录下的ts文件
        include: [resolve('src')]//编译处理的文件夹
      }
    ]
  },

  plugins: [
    new CleanWebpackPlugin({
    }),//清空以前的打包的js

    new HtmlWebpackPlugin({
      template: './public/index.html'   //打包当前的html
    })
  ],

  resolve: {
    extensions: ['.ts', '.tsx', '.js']//对于一下后缀结尾的文件名，可以不写后缀
  },

  devtool: isProd ? 'cheap-module-source-map' : 'cheap-module-eval-source-map',

  devServer: {
    host: 'localhost', // 主机名
    stats: 'errors-only', // 打包日志输出输出错误信息
    port: 8081,
    open: true
  },
}
```
* 在根目录下`npm init -y`生成对应的package.json文件
* `tsc --init`生成对应的tsconfig.json文件

#### 配置打包命令

在package.json文件中：

```json
"scripts":{
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.js",
	"build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
}
```
#### 运行与打包
* yarn包管理器
```powershell
yarn dev
yarn build
```
* npm包管理器
```powershell
npm run dev
npm build
```
********************************

## TypeScript 常用语法
### 1.基础类型
* TypeScript 支持与 JavaScript 几乎相同的数据类型，此外还提供了实用的枚举类型方便我们使用。
#### 布尔值
#### 数字
* 和 JavaScript 一样，**TypeScript 里的所有数字都是浮点数**。 这些浮点数的类型是 number。 除了支持十进制和十六进制字面量，TypeScript 还支持 ECMAScript 2015中引入的二进制和八进制字面量。
```typescript
let a1: number = 10 // 十进制
let a2: number = 0b1010  // 二进制
let a3: number = 0o12 // 八进制
let a4: number = 0xa // 十六进制
```
#### 字符串
#### undefined 和 null
* 默认情况下 `null `和 `undefined `是所有类型的子类型。 就是说你**可以把 `null `和` undefined` 赋值给其他类型的。**
#### 数组
* TypeScript 像 JavaScript 一样可以操作数组元素。 有两种方式可以定义数组。 
  * 第一种，可以在元素类型后面接上[]，表示由此类型元素组成的一个数组：
  ```typescript
  let list1: number[] = [1, 2, 3]
  ```
  * 第二种方式是使用数组泛型，Array<元素类型>：
  ```typescript
  let list2: Array<number> = [1, 2, 3]
  ````
#### 元组 Tuple
* 元组类型允许表示一个已知元素**数量**和**类型**的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 `string `和 `number `类型的元组。
```typescript
let t1: [string, number]
t1 = ['hello', 10] // OK
t1 = [10, 'hello'] // Error
```
* 当访问一个已知索引的元素，会得到正确的类型：
```typescript
console.log(t1[0].substring(1)) // OK
console.log(t1[1].substring(1)) // Error, 'number' 不存在 'substring' 方法
```
> 给元组赋值的时候，参数个数、类型、位置固定，例如第一个必须传递string类型的，就不能传递别的类型

#### 枚举 enum

* `enum` 类型是对 JavaScript 标准数据类型的一个补充。 使用枚举类型可以为一组数值赋予友好的名字。
```typescript
enum Color {
  红,
  Green,
  Blue
}

// 枚举数值默认从0开始依次递增
// 根据特定的名称得到对应的枚举数值
let myColor: Color = Color.Green  // 0
console.log(myColor, Color.红, Color.Blue)
```
* 默认情况下，从 0 开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1 开始编号：
```typescript
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green
```
* 或者，全部都采用手动赋值：
```typescript
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green
```
* 枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为 2，但是不确定它映射到 Color 里的哪个名字，我们可以查找相应的名字：
```typescript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2]

console.log(colorName)  // 'Green'
```
#### any
* 有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any 类型来标记这些变量：
```typescript
let notSure: any = 4
notSure = 'maybe a string'
notSure = false // 也可以是个 boolean
```
* 在对现有代码进行改写的时候，`any` 类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。并且当你只知道一部分数据的类型时，`any` 类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：
```typescript
let list: any[] = [1, true, 'free']

list[1] = 100
```
> 不清楚变量类习惯的时候使用any类型
>
> 但是这种情况下没有错误提示信息

#### void

* 某种程度上来说，`void `类型像是与 `any `类型相反，它表示没有任何类型。 **当一个函数没有返回值时，你通常会见到其返回值类型是 void**：
```typescript
/* 表示没有任何类型, 一般用来说明函数的返回值不能是undefined和null之外的值 */
function fn(): void {
  console.log('fn()')
  // return undefined
  // return null
  // return
  // return 1 // error
}
```
>返回值是  undefined和null或什么也不返回默认为没有返回值

* 声明一个` void `类型的变量没有什么大用，因为你只能为它赋予` undefined` 和` null`：
```typescript
let unusable: void = undefined
```
#### object

* `object` 表示非原始类型，也就是除 `number`，`string`，`boolean`之外的类型。

* 使用 `object` 类型，就可以更好的表示像 `Object.create` 这样的 `API`。例如：

```typescript
function fn2(obj:object):object {
  console.log('fn2()', obj)
  return {}
  // return undefined
  // return null
}
console.log(fn2(new String('abc')))
// console.log(fn2('abc') // error
console.log(fn2(String))
```



#### 联合类型

* 联合类型（Union Types）表示取值可以为多种类型中的一种

* 需求1: 定义一个函数,参数类型是一个数字或字符串值的字符串形式值

  ```typescript
  function toString2(x: number | string) : string {
    return x.toString()
  }
  ```

* 需求2: 定义一个一个函数，得到一个数字或字符串值的长度

  ```typescript
  function getLength(x: number | string) {
  
    // return x.length // error
  
    if (x.length) { // error
      return x.length
    } else {
      return x.toString().length
    }
  }
  ```

  

#### 类型断言

* 通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript 会假设你，程序员，已经进行了必须的检查。

* 类型断言有两种形式。 其一是“尖括号”语法, 另一个为 `as` 语法

  ```typescript
  /* 
  类型断言(Type Assertion): 可以用来手动指定一个值的类型
  语法:
      方式一: <类型>值
      方式二: 值 as 类型  tsx中只能用这种方式
  */
  
  /* 需求: 定义一个函数，得到一个字符串或者数值数据的长度 */
  function getLength(x: number | string) {
    if ((<string>x).length) {
      return (x as string).length
    } else {
      return x.toString().length
    }
  }
  console.log(getLength('abcd'), getLength(1234))
  ```

  

#### 类型推断

* 类型推断: TS会在没有明确的指定类型的时候推测出一个类型

* 有下面2种情况: 

  1. 定义变量时赋值了, 推断为对应的类型. 

  2. 定义变量时没有赋值, 推断为any类型

  ```typescript
  /* 定义变量时赋值了, 推断为对应的类型 */
  let b9 = 123 // number
  // b9 = 'abc' // error
  
  /* 定义变量时没有赋值, 推断为any类型 */
  let b10  // any类型
  b10 = 123
  b10 = 'abc'
  ```

  

### 2. 接口

* TypeScript 的核心原则之一是**对值所具有的结构进行类型检查**。我们使用接口（Interfaces）来定义对象的类型。`接口是对象的状态(属性)和行为(方法)的抽象(描述)`

#### 接口初探

* 需求: 创建人的对象, 需要对人的属性进行一定的约束

  ```
  id是number类型, 必须有, 只读的
  name是string类型, 必须有
  age是number类型, 必须有
  sex是string类型, 可以没有
  ```

  

* 下面通过一个简单示例来观察接口是如何工作的：

  ```typescript
  /* 
  在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型
  接口: 是对象的状态(属性)和行为(方法)的抽象(描述)
  接口类型的对象
      多了或者少了属性是不允许的
      可选属性: ?
      只读属性: readonly
  */
  
  // 定义人的接口
  interface IPerson {
    id: number
    name: string
    age: number
    sex: string
  }
  
  const person1: IPerson = {
    id: 1,
    name: 'tom',
    age: 20,
    sex: '男'
  }
  ```
  
* 类型检查器会查看对象内部的属性是否与IPerson接口描述一致, 如果不一致就会提示类型错误

#### 可选属性

* 接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。

  ```typescript
  interface IPerson {
    id: number
    name: string
    age: number
    sex?: string
  }
  ```

* 带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个 `?` 符号。

  可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误。

  ```typescript
  const person2: IPerson = {
    id: 1,
    name: 'tom',
    age: 20,
    // sex: '男' // 可以没有
  }
  ```

#### 只读属性

* 一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 `readonly` 来指定只读属性:

  ```typescript
  interface IPerson {
    readonly id: number//只读
    name: string
    age: number
    sex?: string//可选
  }
  ```

* 一旦赋值后再也不能被改变了。

  ```typescript
  const person2: IPerson = {
    id: 2,
    name: 'tom',
    age: 20,
    // sex: '男' // 可以没有
    // xxx: 12 // error 没有在接口中定义, 不能有
  }
  person2.id = 2 // error
  ```

####  readonly  vs  const

* 最简单判断该用 `readonly` 还是 `const` 的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 `const`，若做为属性则使用 `readonly`。

#### 函数类型

* 接口能够描述 JavaScript 中对象拥有的各种各样的外形。 除了描述带有属性的普通对象外，接口也可以描述函数类型。

* 为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```typescript
/* 
接口可以描述函数类型(参数的类型与返回的类型)
*/

interface SearchFunc {
  (source: string, subString: string): boolean
}
```

* 这样定义后，我们可以像使用其它接口一样使用这个函数类型的接口。 下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。

  ```typescript
  const mySearch: SearchFunc = function (source: string, sub: string): boolean {
    return source.search(sub) > -1
  }
  
  console.log(mySearch('abcd', 'bc'))
  ```

####  类类型

* #### 类实现接口

  * 与 C# 或 Java 里接口的基本作用一样，TypeScript 也能够用它来明确的强制一个类去符合某种契约。

    ```typescript
    /* 
    类类型: 实现接口
    1. 一个类可以实现多个接口
    2. 一个接口可以继承多个接口
    */
    
    interface Alarm {
      alert(): any;
    }
    
    interface Light {
      lightOn(): void;
      lightOff(): void;
    }
    //Alarm接口约束了Car类
    class Car implements Alarm {
      alert() {
          console.log('Car alert');
      }
    }
    
    ```

* ####  一个类可以实现多个接口

```typescript
class Car2 implements Alarm, Light {
    //实现接口中的方法
  alert() {
    console.log('Car alert');
  }
  lightOn() {
    console.log('Car light on');
  }
  lightOff() {
    console.log('Car light off');
  }
}
//实例化类
const car2=new Car2
//调用类中的方法
car2.alert()
```

> 接口中的内容都需要真正的实现

* ####  接口继承接口

  * 和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

    ```typescript
    interface LightableAlarm extends Alarm, Light {
    
    }
    ```


>接口与接口之间叫继承，使用的关键字是  extends
>
>类与接口之间叫实现，使用的关键字是  implements

###  3. 类

* 对于传统的 JavaScript 程序我们会使用`函数`和`基于原型的继承`来创建可重用的组件，但对于熟悉使用面向对象方式的程序员使用这些语法就有些棘手，因为他们用的是`基于类的继承`并且对象是由类构建出来的。 从 ECMAScript 2015，也就是 ES6 开始， JavaScript 程序员将能够使用基于类的面向对象的方式。 使用 TypeScript，我们允许开发者现在就使用这些特性，并且编译后的 JavaScript 可以在所有主流浏览器和平台上运行，而不需要等到下个 JavaScript 版本。

  ```
  class 类名 {
  
  	// 声明属性
  	
  	// 构造方法
  	
  	// 定义实例方法
  }
  ```

  

  >类：可以理解为模板，通过模板可以实例化对象
  >
  >构造函数(constructor)：在实例化对象的时候，可以直接对属性的值进行初始化



####  基本示例



* 下面看一个使用类的例子：

  ```typescript
  /* 
  类的基本定义与使用
  */
  
  class Greeter {
    // 声明属性
    message: string
  
    // 构造方法
    constructor (message: string) {
      //更新对象中的属性数据  
      this.message = message
    }
  
    // 定义实例方法
    greet (): string {
      return 'Hello ' + this.message
    }
  }
  
  // 创建类的实例
  const greeter = new Greeter('world')
  // 调用实例的方法
  console.log(greeter.greet())
  ```

* 如果你使用过 C# 或 Java，你会对这种语法非常熟悉。 我们声明一个 `Greeter` 类。这个类有 3 个成员：一个叫做 `message` 的属性，一个构造函数和一个 `greet` 方法。

  你会注意到，我们在引用任何一个类成员的时候都用了 `this`。 它表示我们访问的是类的成员。

  后面一行，我们使用 `new` 构造了 `Greeter` 类的一个实例。它会调用之前定义的构造函数，创建一个 `Greeter` 类型的新对象，并执行构造函数初始化它。

  最后一行通过 `greeter` 对象调用其 `greet` 方法

#### 继承与多态  

> 如果 A 类继承了 B 类，那么A类叫子类(派生类)，B类叫基类(父类、超类)

* 在 TypeScript 里，我们可以使用常用的面向对象模式。 基于类的程序设计中一种最基本的模式是允许使用继承来扩展现有的类。

* 看下面的例子：

  ```typescript
  /* 
  类的继承
  */
  
  class Animal {
    run (distance: number) {
      console.log(`Animal run ${distance}m`)
    }
  }
  
  class Dog extends Animal {
    cry () {
      console.log('wang! wang!')
    }
  }
  
  const dog = new Dog()
  dog.cry() 
  dog.run(100) // 可以调用从父中继承得到的方法
  ```

  

  * 这个例子展示了最基本的继承：类从基类中继承了属性和方法。 这里，`Dog` 是一个 派生类，它派生自 `Animal` 基类，通过 `extends` 关键字。 派生类通常被称作*子类*，基类通常被称作*超类*。
  
* 因为 `Dog` 继承了 `Animal` 的功能，因此我们可以创建一个 `Dog` 的实例，它能够 `cry()` 和 `run()`。
  
* 下面我们来看个更加复杂的例子。
  
    ```typescript
    class Animal {
      name: string
      
      constructor (name: string) {
        this.name = name
      }
    
      run (distance: number=0) {
        console.log(`${this.name} run ${distance}m`)
      }
    
    }
    
    class Snake extends Animal {
      constructor (name: string) {
        // 调用父类型构造方法
        super(name)
      }
    
      // 重写父类型的方法
      run (distance: number=5) {
        console.log('sliding...')
        super.run(distance)
      }
    }
    
    class Horse extends Animal {
      constructor (name: string) {
        // 调用父类型构造方法
        super(name)
      }
    
      // 重写父类型的方法
      run (distance: number=50) {
        console.log('dashing...')
        // 调用父类型的一般方法
        super.run(distance)
      }
    
      xxx () {
        console.log('xxx()')
      }
    }
    
    const snake = new Snake('sn')
    snake.run()
    
    const horse = new Horse('ho')
    horse.run()
    
    // 父类型引用指向子类型的实例 ==> 多态
    const tom: Animal = new Horse('ho22')
    tom.run()
    
    /* 如果子类型没有扩展的方法, 可以让子类型引用指向父类型的实例 */
    const tom3: Snake = new Animal('tom3')
    tom3.run()
    /* 如果子类型有扩展的方法, 不能让子类型引用指向父类型的实例 */
    // const tom2: Horse = new Animal('tom2')
    // tom2.run()
  ```
  
  >constructor (name: string) {
    >    	// 调用父类型构造方法使用super
  >    	super(name)
    >  }
  >
    >//调用父类中的普通方法
  >
    >super.函数名(参数)
  
  * 这个例子展示了一些上面没有提到的特性。 这一次，我们使用 `extends` 关键字创建了 Animal的两个子类：`Horse` 和 `Snake`。
  
    
  
  * 与前一个例子的不同点是，派生类包含了一个构造函数，它 必须调用 `super()`，它会执行基类的构造函数。 而且，在构造函数里访问 `this` 的属性之前，我们 一定要调用 `super()`。 这个是 TypeScript 强制执行的一条重要规则。
  
  * 这个例子演示了如何在子类里可以重写父类的方法。`Snake`类和 `Horse` 类都创建了 `run` 方法，它们重写了从 `Animal` 继承来的 `run` 方法，使得 `run` 方法根据不同的类而具有不同的功能。注意，即使 `tom` 被声明为 `Animal` 类型，但因为它的值是 `Horse`，调用 `tom.run(34)` 时，它会调用 `Horse` 里重写的方法。
  
    ```
    sliding...
    sn run 5m
    dashing...
    ho run 50m
    ```

#### 公共，私有与受保护的修饰符

* #### 默认为 public

  * 在上面的例子里，我们可以自由的访问程序里定义的成员。 如果你对其它语言中的类比较了解，就会注意到我们在之前的代码里并没有使用 `public` 来做修饰；例如，C# 要求必须明确地使用 `public` 指定成员是可见的。 在 TypeScript 里，成员都默认为 `public`。

  * 你也可以明确的将一个成员标记成 `public`。 我们可以用下面的方式来重写上面的 `Animal` 类

* #### 理解 private

  * 当成员被标记成 `private` 时，它就不能在声明它的类的外部访问。

* #### 理解 protected

  * `protected` 修饰符与 `private` 修饰符的行为很相似，但有一点不同，**`protected`成员在派生类中仍然可以访问**。

  * 例如：

    ```typescript
    /* 
    访问修饰符: 用来描述类内部的属性/方法的可访问性
      public: 默认值, 公开的外部也可以访问
      private: 只能类内部可以访问
      protected: 类内部和子类可以访问
    */
    
    class Animal {
      public name: string
    
      public constructor (name: string) {
        this.name = name
      }
    
      public run (distance: number=0) {
        console.log(`${this.name} run ${distance}m`)
      }
    }
    
    class Person extends Animal {
      private age: number = 18
      protected sex: string = '男'
    
      run (distance: number=5) {
        console.log('Person jumping...')
        super.run(distance)
      }
    }
    
    class Student extends Person {
      run (distance: number=6) {
        console.log('Student jumping...')
    
        console.log(this.sex) // 子类能看到父类中受保护的成员
        // console.log(this.age) //  子类看不到父类中私有的成员
    
        super.run(distance)
      }
    }
    
    console.log(new Person('abc').name) // 公开的可见
    // console.log(new Person('abc').sex) // 受保护的不可见
    // console.log(new Person('abc').age) //  私有的不可见
    ```

#### readonly 修饰符

* 你可以使用 `readonly` 关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。

  ```typescript
  class Person {
    readonly name: string = 'abc'
    constructor(name: string) {
      //修改name的值
      this.name = name
    }
  }
  
  let john = new Person('John')
  // john.name = 'peter' // error
  ```

  > readonly修饰的成员，不能在外部随意修改，但是可以在构造函数中进行修改

* #### readonly 修饰构造函数中的参数属性

* 在上面的例子中，我们必须在 `Person` 类里定义一个只读成员 `name` 和一个参数为 `name` 的构造函数，并且立刻将 `name` 的值赋给 `this.name`，这种情况经常会遇到。 参数属性可以方便地让我们在一个地方定义并初始化一个成员。 下面的例子是对之前 `Person` 类的修改版，使用了参数属性：

  ```typescript
  class Person2 {
    constructor(readonly name: string='哈哈哈') {
        //this.name=name  //这里的this.name主要是为了访问Person2中的name属性
    }
  }
  
  const p = new Person2('jack')
  console.log(p.name)
```
  
* 注意看我们是如何舍弃参数 `name`，仅在构造函数里使用 `readonly name: string` 参数来创建和初始化 `name` 成员。 我们把声明和赋值合并至一处。
  
  
  
  * 参数属性通过给构造函数参数前面添加一个访问限定符来声明。使用 `private` 限定一个参数属性会声明并初始化一个私有成员；对于 `public` 和 `protected` 来说也是一样

> 构造函数中的name参数，一旦使用readonly进行修饰后，那么该name参数可以叫参数属性
>
> 构造函数中的name参数，一旦使用readonly进行修饰后，那么Person2中就有一个属性成员
>
> 构造函数中的name参数，一旦使用readonly进行修饰后，那么外部无法修改其中name的属性成员值
>
> 构造函数中的name参数，一旦使用public进行修饰后，那么Person2中就有一个公共得name属性成员，外部可以修改(private、protected同理)

#### 存取器

* `TypeScript` 支持**通过 `getters/setters` 来截取对对象成员的访问**。 它能帮助你**有效的控制对对象成员的访问。**

* 下面来看如何把一个简单的类改写成使用 `get` 和 `set`。 首先，我们从一个没有使用存取器的例子开始。

```typescript
class Person {
  firstName: string = 'A'
  lastName: string = 'B'
  // 读取器：负责读取数据
  get fullName () {
    return this.firstName + '-' + this.lastName
  }
  // 设置器：负责设置修改数据
  set fullName (value) {
    const names = value.split('-')
    this.firstName = names[0]
    this.lastName = names[1]
  }
}

const p = new Person()
console.log(p.fullName)

p.firstName = 'C'
p.lastName =  'D'
//读取数据
console.log(p.fullName)
//设置数据  
p.fullName = 'E-F'
console.log(p.firstName, p.lastName)
```

#### 静态属性 static

* 到目前为止，我们只讨论了类的实例成员，那些仅当类被实例化的时候才会被初始化的属性。 我们也可以创建类的静态成员，这些属性存在于类本身上面而不是类的实例上。 在这个例子里，我们使用 `static` 定义 `origin`，因为它是所有网格都会用到的属性。 每个实例想要访问这个属性的时候，都要在 `origin` 前面加上类名。 如同在实例属性上使用 `this.xxx` 来访问属性一样，这里我们使用 `Grid.xxx` 来访问静态属性。

```typescript
/* 
静态属性, 是类对象的属性
非静态属性, 是类的实例对象的属性
*/

class Person {
  name1: string = 'A'
  static name2: string = 'B'
  static sayHi(){
      console.log('111')
  }
}

console.log(Person.name2)
console.log(Person.sayHi())
console.log(new Person().name1)
```

> static修饰得属性或方法叫静态成员(静态方法)
>
> static不能出现在构造函数中

#### 抽象类 abstract

* 抽象类做为其它派生类的基类使用。 它们不能被实例化。不同于接口，抽象类可以包含成员的实现细节。 `abstract` 关键字是用于定义抽象类和在抽象类内部定义抽象方法。

  ```typescript
  /* 
  抽象类
    不能创建实例对象, 只有实现类才能创建实例
    可以包含未实现的抽象方法
  */
  
  //抽象类
  abstract class Animal {
  
    abstract cry ()
  
    run () {
      console.log('run()')
    }
  }
  //派生类
  class Dog extends Animal {
    cry () {
      console.log(' Dog cry()')
    }
  }
  //抽象类不能被实例化（不能用new...）
  const animal = new Dog()
  const dog = new Dog()
  //调用抽象类中的方法 9 
dog.cry()
  dog.run()
  ```
  
  > 抽象类：包含抽象方法（抽象方法一般没有任何具体的实现），也可以包含实例方法，抽象类不能被实例化，为了让字类继续宁实例化及实现内部抽象方法

###  4. 函数

* 函数是 JavaScript 应用程序的基础，它帮助你实现抽象层，模拟类，信息隐藏和模块。在 TypeScript 里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义行为的地方。TypeScript 为 JavaScript 函数添加了额外的功能，让我们可以更容易地使用。

#### 基本示例

* 和 JavaScript 一样，TypeScript 函数可以创建有名字的函数和匿名函数。你可以随意选择适合应用程序的方式，不论是定义一系列 API 函数还是只使用一次的函数。

* 通过下面的例子可以迅速回想起这两种 JavaScript 中的函数：

  ```typescript
  // 命名函数
  function add(x, y) {
    return x + y
  }
  
  // 匿名函数
  let myAdd = function(x, y) { 
    return x + y;
  }
  ```

#### 函数类型

* #### 为函数定义类型

* 让我们为上面那个函数添加类型：

  ```typescript
  function add(x: number, y: number): number {
    return x + y
  }
  
  let myAdd = function(x: number, y: number): number { 
    return x + y
  }
  ```

  * 我们可以给每个参数添加类型之后再为函数本身添加返回值类型。TypeScript 能够根据返回语句自动推断出返回值类型。         

* #### 书写完整函数类型

* 现在我们已经为函数指定了类型，下面让我们写出函数的完整类型。

  ```typescript
  let myAdd2: (x: number, y: number) => number = 
  function(x: number, y: number): number {
    return x + y
  }
  ```



#### 可选参数和默认参数

* TypeScript 里的每个函数参数都是必须的。 这不是指不能传递 `null` 或 `undefined` 作为参数，而是说编译器检查用户是否为每个参数都传入了值。编译器还会假设只有这些参数会被传递进函数。 简短地说，传递给一个函数的参数个数必须与函数期望的参数个数一致。

* JavaScript 里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是 `undefined`。 在TypeScript 里我们可以在参数名旁使用 `?` 实现可选参数的功能。 比如，我们想让 `lastName` 是可选的：

* 在 TypeScript 里，我们也可以为参数提供一个默认值当用户没有传递这个参数或传递的值是 `undefined` 时。 它们叫做有默认初始化值的参数。 让我们修改上例，把`firstName` 的默认值设置为 `"A"`。

  ```typescript
  function buildName(firstName: string='A', lastName?: string): string {
    if (lastName) {
      return firstName + '-' + lastName
    } else {
      return firstName
    }
  }
  
  console.log(buildName('C', 'D'))
  console.log(buildName('C'))
  console.log(buildName())
  ```

* #### 剩余参数  args 

* 必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在 JavaScript 里，你可以使用 `arguments` 来访问所有传入的参数。

* 在 TypeScript 里，你可以把所有参数收集到一个变量里：
  剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个。 编译器创建参数数组，名字是你在省略号（ `...`）后面给定的名字，你可以在函数体内使用这个数组。

```typescript
function info(x: string, ...args: string[]) {
  console.log(x, args)
}
info('abc', 'c', 'b', 'a')
```

#### 函数重载

* 函数重载: **函数名相同, 而形参不同的多个函数**

* 在JS中, 由于弱类型的特点和形参与实参可以不匹配, 是没有函数重载这一说的 但在TS中, 与其它面向对象的语言(如Java)就存在此语法

  ```typescript
  /* 
  函数重载: 函数名相同, 而形参不同的多个函数
  需求: 我们有一个add函数，它可以接收2个string类型的参数进行拼接，也可以接收2个number类型的参数进行相加 
  */
  
  // 重载函数声明
  //参数x类型为string、y类型为string、返回值是string类型
  function add (x: string, y: string): string
  //参数x类型为number、y类型为number、返回值是number类型
  function add (x: number, y: number): number
  
  // 定义函数实现
  function add(x: string | number, y: string | number): string | number {
    // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 x + y
    if (typeof x === 'string' && typeof y === 'string') {
      return x + y
    } else if (typeof x === 'number' && typeof y === 'number') {
      return x + y
    }
  }
  
  console.log(add(1, 2))
  console.log(add('a', 'b'))
// console.log(add(1, 'a')) // error
  ```
  
  

### 5. 泛型

* 在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定具体类型的一种特性

#### 引入

* 下面创建一个函数, 实现功能: 根据指定的数量 `count` 和数据 `value` , 创建一个包含 `count` 个 `value` 的数组 不用泛型的话，这个函数可能是下面这样：

  ```typescript
  function createArray(value: any, count: number): any[] {
    const arr: any[] = []
    for (let index = 0; index < count; index++) {
      arr.push(value)
    }
    return arr
  }
  
  const arr1 = createArray(11, 3)
  const arr2 = createArray('aa', 3)
  console.log(arr1[0].toFixed(), arr2[0].split(''))
  ```

#### 使用函数泛型

```typescript
//T是随意命名的
function createArray2 <T> (value: T, count: number) {
  const arr: Array<T> = []
  for (let index = 0; index < count; index++) {
    arr.push(value)
  }
  return arr
}
//实例化的时候对T进行赋值
const arr3 = createArray2<number>(11, 3)
console.log(arr3[0].toFixed())
// console.log(arr3[0].split('')) // error
const arr4 = createArray2<string>('aa', 3)
console.log(arr4[0].split(''))
// console.log(arr4[0].toFixed()) // error
```

#### 多个泛型参数的函数

* 一个函数可以定义多个泛型参数

  ```typescript
  function swap <K, V> (a: K, b: V): [K, V] {
    return [a, b]
  }
  const result = swap<string, number>('abc', 123)
  console.log(result[0].length, result[1].toFixed())
  ```

#### 泛型接口

* 在定义接口时, 为接口中的属性或方法定义泛型类型
* 在使用接口时, 再指定具体的泛型类型

```typescript
interface IbaseCRUD <T> {
  data: T[]
  add: (t: T) => void
  getById: (id: number) => T
}

class User {
  id?: number; //id主键自增
  name: string; //姓名
  age: number; //年龄

  constructor (name, age) {
    this.name = name
    this.age = age
  }
}

class UserCRUD implements IbaseCRUD <User> {
  data: User[] = []
  
  add(user: User): void {
    user = {...user, id: Date.now()}
    this.data.push(user)
    console.log('保存user', user.id)
  }

  getById(id: number): User {
    return this.data.find(item => item.id===id)
  }
}


const userCRUD = new UserCRUD()
userCRUD.add(new User('tom', 12))
userCRUD.add(new User('tom2', 13))
console.log(userCRUD.data)
```

#### 泛型类

* 在定义类时, 为类中的属性或方法定义泛型类型 在创建类的实例时, 再指定特定的泛型类型

  ```typescript
  class GenericNumber<T> {
    zeroValue: T
    add: (x: T, y: T) => T
  }
  
  let myGenericNumber = new GenericNumber<number>()
  myGenericNumber.zeroValue = 0
  myGenericNumber.add = function(x, y) {
    return x + y 
  }
  
  let myGenericString = new GenericNumber<string>()
  myGenericString.zeroValue = 'abc'
  myGenericString.add = function(x, y) { 
    return x + y
  }
  
  console.log(myGenericString.add(myGenericString.zeroValue, 'test'))
  console.log(myGenericNumber.add(myGenericNumber.zeroValue, 12))
  ```

#### 泛型约束

* 如果我们直接对一个泛型参数取 `length` 属性, 会报错, 因为这个泛型根本就不知道它有这个属性

  ```typescript
  // 没有泛型约束
  function fn <T>(x: T): void {
    // console.log(x.length)  // error
  }
  ```

* 我们可以使用泛型约束来实现

  ```typescript
  interface Lengthwise {
    length: number;
  }
  
  // 指定泛型约束
  function fn2 <T extends Lengthwise>(x: T): void {
    console.log(x.length)
  }
  ```

* 我们需要传入符合约束类型的值，必须包含必须 `length` 属性：

```typescript
fn2('abc')
// fn2(123) // error  number没有length属性
```



###  6.其它

#### 声明文件

* 当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能

什么是声明语句



* 假如我们想使用第三方库 jQuery，一种常见的方式是在 html 中通过 `<script>` 标签引入 `jQuery`，然后就可以使用全局变量 `$` 或 `jQuery` 了。

  

* 但是在 ts 中，编译器并不知道 $ 或 jQuery 是什么东西

```typescript
/* 
当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。
声明语句: 如果需要ts对新的语法进行检查, 需要要加载了对应的类型说明代码
  declare var jQuery: (selector: string) => any;
声明文件: 把声明语句放到一个单独的文件（jQuery.d.ts）中, ts会自动解析到项目中所有声明文件
下载声明文件: npm install @types/jquery --save-dev
*/

jQuery('#foo');
// ERROR: Cannot find name 'jQuery'.
```

* 这时，我们需要使用 declare var 来定义它的类型

```typescript
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```



* declare var 并没有真的定义一个变量，只是定义了全局变量 jQuery 的类型，仅仅会用于编译时的检查，在编译结果中会被删除。它编译结果是：

  

```typescript
jQuery('#foo');
```



* 一般声明文件都会单独写成一个 `xxx.d.ts` 文件

  创建 `01_jQuery.d.ts`, 将声明语句定义其中, TS编译器会扫描并加载项目中所有的TS声明文件



```typescript
declare var jQuery: (selector: string) => any;
```



* 很多的第三方库都定义了对应的声明文件库, 库文件名一般为 `@types/xxx`, 可以在 `https://www.npmjs.com/package/package` 进行搜索
* 有的第三库在下载时就会自动下载对应的声明文件库(比如: webpack),有的可能需要单独下载(比如jQuery/react)

#### 内置对象

* JavaScript 中有很多内置对象，它们可以直接在 TypeScript 中当做定义好了的类型。

* 内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

  ```typescript
  /* 1. ECMAScript 的内置对象 */
  let b: Boolean = new Boolean(1)
  let n: Number = new Number(true)
  let s: String = new String('abc')
  let d: Date = new Date()
  let r: RegExp = /^1/
  let e: Error = new Error('error message')
  b = true
  // let bb: boolean = new Boolean(2)  // error
  ```

  

* #### BOM与DOM的内置对象

```typescript
const div: HTMLElement = document.getElementById('test')
const divs: NodeList = document.querySelectorAll('div')
document.addEventListener('click', (event: MouseEvent) => {
  console.dir(event.target)
})
const fragment: DocumentFragment = document.createDocumentFragment()
```