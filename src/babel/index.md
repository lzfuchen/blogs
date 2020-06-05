#babel

##babel介绍

基于babel7

babel7将原来的 babel-xx 包统一迁移到babel域下，域由 @ 符号来标识，一来便于区别官方与非官方的包，二来避免可能的包命名冲突

> Babel 是一个工具链，主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

package | 作用
-- | --
@babel/cli|babel提供的内建的命令行工具，主要是提供babel这个命令来对js文件进行编译
@babel/core|babel的核心功能包含在 @babel/core 模块中。不安装 @babel/core，无法使用 babel 进行编译。
@babel/plugin-transform-xxx| 插件用来将es6方法转化为es5方法。比如：@babel/plugin-transform-arrow-functions转化箭头函数
@babel/preset-env|一组插件的集合，包含支持所有最新的JS特性将其转换成ES5代码。会根据你配置的目标环境，生成插件列表来编译。官方推荐使用 .browserslistrc 文件来指定目标环境
@babel/polyfill|已废弃，用core-js@3替代。语法转换只是将高版本的语法转换成低版本的，但是新的内置函数、实例方法无法转换。这时，就需要使用 polyfill 上场了，顾名思义，polyfill的中文意思是垫片，所谓垫片就是垫平不同浏览器或者不同环境下的差异，让新的内置函数、实例方法等在低版本浏览器中也可以使用。
@babel/runtime|提供统一的模块化的helper，那什么是helper，我们举个例子： 我们编译之后的index.js代码里面有不少新增加的函数，如_classCallCheck，_defineProperties，_createClass，这种函数就是helper。
@babel/plugin-transform-runtime| 使所有helper都将引用模块 @babel/runtime，这样就可以避免编译后的代码中出现重复的帮助程序，有效减少包体积。除了减少编译后代码的体积外，我们使用它还有一个好处，它可以为代码创建一个沙盒环境，如果使用 @babel/polyfill 及其提供的内置程序（例如 Promise ，Set 和 Map ），则它们将污染全局范围。虽然这对于应用程序或命令行工具可能是可以的，但是如果你的代码是要发布供他人使用的库，或者无法完全控制代码运行的环境，则将成为一个问题。

## 配置文件

安装packages
```javascript
npm i -D @babel/core @babel/cli @babel/preset-env
npm i -S core-js@3


//babel会使用很小的辅助函数来实现类似 _createClass 等公共方法。默认情况下，它将被添加(inject)到需要它的每个文件中。如果你有10个文件中都使用了这个 class，意味着 _classCallCheck、_defineProperties、_createClass 这些方法被 inject 了10次。这显然会导致包体积增大，最关键的是，我们并不需要它 inject 多次。这个时候就需要使用 @babel/plugin-transform-runtime 插件，所有的helper都将引用模块 @babel/runtime，这样就可以避免编译后的代码中出现重复的帮助程序，有效减少包体积。

//@babel/plugin-transform-runtime 是一个可以重复使用 Babel 注入的帮助程序，以节省代码大小的插件。

//@babel/plugin-transform-runtime 需要和 @babel/runtime 配合使用。
npm i -D @babel/plugin-transform-runtime
npm i -S @babel/runtime
```

创建 .babelrc 文件。内容如下
```javascript
{
  "presets": [
    [
      "@babel/env",
      {
        "modules": false,//一般webpack会将 es6 转为 commonjs 规范。babel就不需要转了
        "useBuiltIns": "usage",
        "corejs": "3"
      }
    ]
  ],
  "plugins": [
    [
      "@babel/transform-runtime",{
        "helpers": true,
        "regenerator": false, //
        "useESModules": true //使用 esm helpers, 减少 commonJS 语法代码 ssr项目需要设置为false，node不支持esm
      }
    ]
  ]
}
```

创建 .browserslistrc 文件。可以根据自己的需求配置，内容如下
```javasript
ie 10
edge 17
firefox 60
chrome 44
safari 7
```

---

如果想为代码创建一个沙盒环境，不污染全局范围我们修改下 babel 的配置：
```javascript
npm i -S @babel/runtime-corejs3
{
  "presets": [
    [
      "@babel/env"
    ]
  ],
  "plugins": [
    [
      "@babel/transform-runtime",{
        "corejs": 3
      }
    ]
  ]
}
```

## @babel/preset-env

属性|作用
--|--
targets|描述项目支持的环境，一般配置在 .browserslistrc 文件中。 postcss会共用这个文件
modules|默认：auto。会把es6代码转换为其他模块类型
useBuiltIns| 默认：false。配置用来告诉env如何使用polyfills。“entry”：需要我们在入口文件自己引入用的polyfills或者全部引入“import '@babel/polyfill'”。全部引入也可以使用webpack“entry: ['@babel/polyfill', './app']”

## @babel/plugin-transform-runtime

属性|作用
--|--
corejs|默认值：false。为数字表示用transform-runtime来提供polyfill，不污染全局
regenerator| 默认值：true。注入一个不污染全局范围的generator函数。
useESModules| 默认值：false。为true表示使用esm的helpers