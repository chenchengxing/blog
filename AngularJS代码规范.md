# 书写具备一致风格、通俗易懂 JavaScript 的原则 -for AngularJS

## Zen
> 无论有多少人在维护，所有在代码仓库中的代码理应看起来像同一个人写的。

> 下面的清单概括了我作为原作者的所有代码中使用的实践。在我创建的项目中的所有构建代码都必须遵循这些规则。

> 我并不想强制别人在其代码或项目中使用我个人所偏好的代码风格；如果已经存在一个通用编码规范，它必须受到尊崇。


> "对风格的挑刺毫无意义可言。它们必须是指导原则，且你必须遵循。"
>_Rebecca_ _Murphey_


>  "成为一个优秀的成功项目管理者的一个条件是，明白按自己的偏好风格写代码是非常不好的做法。如果成千上万的人都在使用你的代码，那么请尽可能通俗易懂地写出你的代码，而非在规范之下自作聪明地使用自己偏好的风格。"
>_Idan_ _Gazit_


## 目录

 * [Whitespace](#whitespace)
 * [Beautiful Syntax](#spacing)
 * [Type Checking (Courtesy jQuery Core Style Guidelines)](#type)
 * [Conditional Evaluation](#cond)
 * [Practical Style](#practical)
 * [Naming](#naming)
 * [Misc](#misc)
 * [Native & Host Objects](#native)
 * [Comments](#comments)
 * [One Language Code](#language)

------------------------------------------------

## 前言

下面的章节描述的是一个 _合理_ 的现代 JavaScript 开发风格指南，并非硬性规定。其想送出的核心理念是*高度统一的代码风格*（the law of code style consistency）。你为项目所择风格都应为最高准则。作为一个描述放置于你的项目中，并链接到这个文档作为代码风格一致性、可读性和可维护性的保证。

## 炸油条 风格宣言

0.使用 单引号' ,如 'i\'m zhayoutiao'
1. <a name="whitespace">空白</a>
  - 永远都不要混用空格和Tab。
  - 开始一个项目，在写代码之前，选择 `空格1 （作为缩进方式），并将其作为**最高准则**。
  - 为了可读, 必须使用2个字母宽度的缩进 &mdash; 这等同于两个空格或者两个空格替代一个 Tab。


2. <a name="spacing">美化语法</a>

    A. 小括号, 花括号, 换行
```javascript

    // if/else/for/while/try 通常都有小括号、花括号和多行
    // 这有助于可读

    // 2.A.1.1
    // 难辨语法（cramped syntax）的例子

    if(condition) doSomething();

    while(condition) iterating++;

    for(var i=0;i<100;i++) someIterativeFn();


    // 2.A.1.1
    // 使用空格来提升可读性

    if ( condition ) {
      // 语句
    }

    while ( condition ) {
      // 语句
    }

    for ( var i = 0; i < 100; i++ ) {
      // 语句
    }

    // 更好的做法:

    var i,
      length = 100;

    for ( i = 0; i < length; i++ ) {
      // 语句
    }

    // 或者...

    var i = 0,
      length = 100;

    for ( ; i < length; i++ ) {
      // 语句
    }

    var prop;

    for ( prop in object ) {
      // 语句
    }


    if ( true ) {
      // 语句
    } else {
      // 语句
    }
    ```


    B. 赋值, 声明, 函数 ( 命名函数, 函数表达式, 构建函数 )

    ```javascript

    // 2.B.1.1
    // 变量
    var foo = "bar",
      num = 1,
      undef;

    // 字面量标识:
    var array = [],
      object = {};


    // 2.B.1.2
    // 在一个作用域（函数）内只使用一个 `var` 有助于提升可读性
    // 并且让你的声明列表变得有条不紊 (还帮你省了几次键盘敲击)

    // 不好
    var foo = "";
    var bar = "";
    var qux;

    // 好
    var foo = "",
      bar = "",
      quux;

    // 或者..
    var // 对这些变量的注释
    foo = "",
    bar = "",
    quux;

    // 2.B.1.3
    // `var` 语句必须总是在各自作用域（函数）顶部
    // 同样适应于来自 ECMAScript 6 的常量

    // 不好
    function foo() {

      // 在变量前有语句

      var bar = "",
        qux;
    }

    // 好
    function foo() {
      var bar = "",
        qux;

      // 所有语句都在变量之后
    }
    ```

    ```javascript

    // 2.B.2.1
    // 命名函数声明
    function foo( arg1, argN ) {

    }

    // 使用方法
    foo( arg1, argN );


    // 2.B.2.2
    // 命名函数声明
    function square( number ) {
      return number * number;
    }

    // 使用方法
    square( 10 );

    // 非常不自然的连带传参（continuation passing）风格
    function square( number, callback ) {
      callback( number * number );
    }

    square( 10, function( square ) {
      // 回调内容
    });


    // 2.B.2.3
    // 函数表达式
    var square = function( number ) {
      // 返回有价值的、相关的内容
      return number * number;
    };

    // 带标识符的函数表达式
    // 这种首选形式有附加的功能让其可以调用自身
    // 并且在堆栈中有标识符
    var factorial = function factorial( number ) {
      if ( number < 2 ) {
        return 1;
      }

      return number * factorial( number-1 );
    };


    // 2.B.2.4
    // 构造函数声明
    function FooBar( options ) {

      this.options = options;
    }

    // 使用方法
    var fooBar = new FooBar({ a: "alpha" });

    fooBar.options;
    // { a: "alpha" }

    ```


    C. 异常, 细节

    ```javascript

    // 2.C.1.1
    // 带回调的函数
    foo(function() {
      // 注意：在第一函数调用的小括号和 `function` 处并没有空格
    });

    // 函数接受 `array` 作为参数，没有空格
    foo([ "alpha", "beta" ]);

    // 2.C.1.2
    // 函数接受 `object` 作为参数，没有空格
    foo({
      a: "alpha",
      b: "beta"
    });

    // 函数接受 `string` 字面量作为参数，没有空格
    foo("bar");

    // 分组用的小括号内部，没有空格
    if ( !("foo" in obj) ) {

    }

    ```


3. <a name="type">类型检测 (来源于 jQuery Core Style Guidelines)</a>

    A. 直接类型（实际类型，Actual Types）

    String:

        typeof variable === "string"

    Number:

        typeof variable === "number"

    Boolean:

        typeof variable === "boolean"

    Object:

        typeof variable === "object"

    Array:

        Array.isArray( arrayLikeObject )
        (如果可能的话)

    Node:

        elem.nodeType === 1

    null:

        variable === null

    null or undefined:

        variable == null

    undefined:

      全局变量:

        typeof variable === "undefined"

      局部变量:

        variable === undefined

      属性:

        object.prop === undefined
        object.hasOwnProperty( prop )
        "prop" in object

    B. 转换类型（强制类型，Coerced Types）

    ```

    对于强制类型转换这里有几个例子:


    ```javascript

    // 3.B.2.1

    var number = 1,
      string = "1",
      bool = false;

    number;
    // 1

    number + "";
    // "1"

    string;
    // "1"

    +string;
    // 1

    +string++;
    // 1

    string;
    // 2

    bool;
    // false

    +bool;
    // 0

    bool + "";
    // "false"
    ```


    ```javascript
    // 3.B.2.2

    var number = 1,
      string = "1",
      bool = true;

    string === number;
    // false

    string === number + "";
    // true

    +string === number;
    // true

    bool === number;
    // false

    +bool === number;
    // true

    bool === string;
    // false

    bool === !!string;
    // true
    ```

    ```javascript
    // 3.B.2.3

    var array = [ "a", "b", "c" ];

    !!~array.indexOf("a");
    // true

    !!~array.indexOf("b");
    // true

    !!~array.indexOf("c");
    // true

    !!~array.indexOf("d");
    // false

    // 值得注意的是上述都是 "不必要的聪明"
    // 采用明确的方案来比较返回的值
    // 如 indexOf：

    if ( array.indexOf( "a" ) >= 0 ) {
      // ...
    }
    ```

    ```javascript
    // 3.B.2.3


    var num = 2.5;

    parseInt( num, 10 );

    // 等价于...

    ~~num;

    num >> 0;

    num >>> 0;

    // 结果都是 2


    // 时刻牢记心底, 负值将被区别对待...

    var neg = -2.5;

    parseInt( neg, 10 );

    // 等价于...

    ~~neg;

    neg >> 0;

    // 结果都是 -2
    // 但是...

    neg >>> 0;

    // 结果即是 4294967294

    ```

4. <a name="cond">对比运算</a>

    ```javascript

    // 4.1.1
    // 当只是判断一个 array 是否有长度，相对于使用这个:
    if ( array.length > 0 ) ...

    // ...判断真伪, 请使用这种:
    if ( array.length ) ...


    // 4.1.2
    // 当只是判断一个 array 是否为空，相对于使用这个:
    if ( array.length === 0 ) ...

    // ...判断真伪, 请使用这种:
    if ( !array.length ) ...


    // 4.1.3
    // 当只是判断一个 string 是否为空，相对于使用这个:
    if ( string !== "" ) ...

    // ...判断真伪, 请使用这种:
    if ( string ) ...


    // 4.1.4
    // 当只是判断一个 string 是为空，相对于使用这个:
    if ( string === "" ) ...

    // ...判断真伪, 请使用这种:
    if ( !string ) ...


    // 4.1.5
    // 当只是判断一个引用是为真，相对于使用这个:
    if ( foo === true ) ...

    // ...判断只需像你所想，享受内置功能的好处:
    if ( foo ) ...


    // 4.1.6
    // 当只是判断一个引用是为假，相对于使用这个:
    if ( foo === false ) ...

    // ...使用叹号将其转换为真
    if ( !foo ) ...

    // ...需要注意的是：这个将会匹配 0, "", null, undefined, NaN
    // 如果你 _必须_ 是布尔类型的 false，请这样用：
    if ( foo === false ) ...


    // 4.1.7
    // 如果想计算一个引用可能是 null 或者 undefined，但并不是 false, "" 或者 0,
    // 相对于使用这个：
    if ( foo === null || foo === undefined ) ...

    // ...享受 == 类型强制转换的好处，像这样:
    if ( foo == null ) ...

    // 谨记，使用 == 将会令 `null` 匹配 `null` 和 `undefined`
    // 但不是 `false`，"" 或者 0
    null == undefined

    ```
    总是判断最好、最精确的值，上述是指南而非教条。

    ```javascript

    // 4.2.1
    // 类型转换和对比运算说明

    // 首次 `===`，`==` 次之 (除非需要松散类型的对比)

    // `===` 总不做类型转换，这意味着:

    "1" === 1;
    // false

    // `==` 会转换类型，这意味着:

    "1" == 1;
    // true


    // 4.2.2
    // 布尔, 真 & 伪

    // 布尔:
    true, false

    // 真:
    "foo", 1

    // 伪:
    "", 0, null, undefined, NaN, void 0

    ```


5. <a name="practical">实用风格</a>

    ```javascript

    // 5.1.1
    // 一个实用的模块

    (function( global ) {
      var Module = (function() {

        var data = "secret";

        return {
          // 这是一个布尔值
          bool: true,
          // 一个字符串
          string: "a string",
          // 一个数组
          array: [ 1, 2, 3, 4 ],
          // 一个对象
          object: {
            lang: "en-Us"
          },
          getData: function() {
            // 得到 `data` 的值
            return data;
          },
          setData: function( value ) {
            // 返回赋值过的 `data` 的值
            return ( data = value );
          }
        };
      })();

      // 其他一些将会出现在这里

      // 把你的模块变成全局对象
      global.Module = Module;

    })( this );

    ```

    ```javascript

    // 5.2.1
    // 一个实用的构建函数

    (function( global ) {

      function Ctor( foo ) {

        this.foo = foo;

        return this;
      }

      Ctor.prototype.getFoo = function() {
        return this.foo;
      };

      Ctor.prototype.setFoo = function( val ) {
        return ( this.foo = val );
      };


      // 不使用 `new` 来调用构建函数，你可能会这样做：
      var ctor = function( foo ) {
        return new Ctor( foo );
      };


      // 把我们的构建函数变成全局对象
      global.ctor = ctor;

    })( this );

    ```



6. <a name="naming">命名</a>


    A. 你并不是一个人肉 编译器/压缩器，所以尝试去变身为其一。

    下面的代码是一个极糟命名的典范:

    ```javascript

    // 6.A.1.1
    // 糟糕命名的示例代码

    function q(s) {
      return document.querySelectorAll(s);
    }
    var i,a=[],els=q("#foo");
    for(i=0;i<els.length;i++){a.push(els[i]);}
    ```

    毫无疑问，你写过这样的代码 —— 希望从今天它不再出现。

    这里有一份相同逻辑的代码，但拥有更健壮、贴切的命名（和一个可读的结构）：

    ```javascript

    // 6.A.2.1
    // 改善过命名的示例代码

    function query( selector ) {
      return document.querySelectorAll( selector );
    }

    var idx = 0,
      elements = [],
      matches = query("#foo"),
      length = matches.length;

    for ( ; idx < length; idx++ ) {
      elements.push( matches[ idx ] );
    }

    ```

    一些额外的命名提示：

    ```javascript

    // 6.A.3.1
    // 命名字符串

    `dog` 是一个 string


    // 6.A.3.2
    // 命名 arrays

    `['dogs']` 是一个包含 `dog 字符串的 array


    // 6.A.3.3
    // 命名函数、对象、实例，等

    camlCase; function 和 var 声明


    // 6.A.3.4
    // 命名构建器、原型，等

    PascalCase; 构建函数


    // 6.A.3.5
    // 命名正则表达式

    rDesc = //;


    // 6.A.3.6
    // 来自 Google Closure Library Style Guide

    functionNamesLikeThis;
    variableNamesLikeThis;
    ConstructorNamesLikeThis;
    EnumNamesLikeThis;
    methodNamesLikeThis;
    SYMBOLIC_CONSTANTS_LIKE_THIS;

    ```

  

7. <a name="misc">Misc</a>

    这个部分将要说明的想法和理念都并非教条。相反更鼓励对现存实践保持好奇，以尝试提供完成一般 JavaScript 编程任务的更好方案。

    A. 避免使用 `switch`，现代方法跟踪（method tracing）将会把带有 switch 表达式的函数列为黑名单。

    似乎在最新版本的 Firefox 和 Chrome 都对 `switch` 语句有重大改进。http://jsperf.com/switch-vs-object-literal-vs-module

    值得注意的是，改进可以这里看到:
    https://github.com/rwldrn/idiomatic.js/issues/13

    ```javascript

    // 7.A.1.1
    // switch 语句示例

    switch( foo ) {
      case "alpha":
        alpha();
        break;
      case "beta":
        beta();
        break;
      default:
        // 默认分支
        break;
    }

    // 7.A.1.2
    // 一个可支持组合、重用的方法是使用一个对象来存储 “cases”，
    // 使用一个 function 来做委派：

    var cases, delegator;

    // 返回值仅作说明用
    cases = {
      alpha: function() {
        // 语句
        // 一个返回值
        return [ "Alpha", arguments.length ];
      },
      beta: function() {
        // 语句
        // 一个返回值
        return [ "Beta", arguments.length ];
      },
      _default: function() {
        // 语句
        // 一个返回值
        return [ "Default", arguments.length ];
      }
    };

    delegator = function() {
      var args, key, delegate;

      // 把 `argument` 转换成数组
      args = [].slice.call( arguments );

      // 从 `argument` 中抽出最前一个值
      key = args.shift();

      // 调用默认分支
      delegate = cases._default;

      // 从对象中对方法进行委派操作
      if ( cases.hasOwnProperty( key ) ) {
        delegate = cases[ key ];
      }

      // arg 的作用域可以设置成特定值，
      // 这种情况下，|null| 就可以了
      return delegate.apply( null, args );
    };

    // 7.A.1.3
    // 使用 7.A.1.2 中的 API:

    delegator( "alpha", 1, 2, 3, 4, 5 );
    // [ "Alpha", 5 ]

    // 当然 `case` key 的值可以轻松地换成任意值

    var caseKey, someUserInput;

    // 有没有可能是某种形式的输入?
    someUserInput = 9;

    if ( someUserInput > 10 ) {
      caseKey = "alpha";
    } else {
      caseKey = "beta";
    }

    // 或者...

    caseKey = someUserInput > 10 ? "alpha" : "beta";

    // 然后...

    delegator( caseKey, someUserInput );
    // [ "Beta", 1 ]

    // 当然还可以这样搞...

    delegator();
    // [ "Default", 0 ]


    ```

    B. 提前返回值提升代码的可读性并且没有太多性能上的差别

    ```javascript

    // 7.B.1.1
    // 不好:
    function returnLate( foo ) {
      var ret;

      if ( foo ) {
        ret = "foo";
      } else {
        ret = "quux";
      }
      return ret;
    }

    // 好:

    function returnEarly( foo ) {

      if ( foo ) {
        return "foo";
      }
      return "quux";
    }

    ```



9. <a name="comments">注释</a>

  * 单行注释放于代码上方为首选
  * 多行也可以
  * 行末注释应被避免!
  * JSDoc 的方式也不错，但需要比较多的时间


