## 开始

突然想阅读下[colors](https://github.com/Marak/colors.js)的源码，很好奇为什么在console里面可以输出彩色的字体，后来才发现坑很深。

在style.js里，是这样构造输出的字符串的。
```js
Object.keys(codes).forEach(function (key) {
  var val = codes[key];
  var style = styles[key] = [];
  style.open = '\u001b[' + val[0] + 'm';
  style.close = '\u001b[' + val[1] + 'm';
});
```

 其中codes是颜色的map，最终会调用stylize函数，把最终的输出拼凑起来。

 ```js
 var stylize = colors.stylize = function stylize (str, style) {
  return ansiStyles[style].open + str + ansiStyles[style].close;
}
 ```
那么`\u001b[`这些又是什么鬼。

首先，得知道`\u`。

`\u`是javascript escape的一种形式。

- \001 八进制转义序列 字符码256内 最大为 \377
- \xA9 十六进制转义序列 字符码256内 最大为 \xFF
- \uABCD Unicode转义序列 字符码256*256内 最大为 \uFFFF
- JSON格式中唯一接受的是Unicode转义序列

好，那\u001b代表一个字符，我们去查下[ASCII表](http://en.wikipedia.org/wiki/ASCII)就知道了是代表ESC。

这里我们又要去了解[ANSI_escape_code](http://en.wikipedia.org/wiki/ANSI_escape_code#Colors)。

｀ESC[｀代表转义的起始符，也就是大家约定，以这玩意开头的都有特殊的含义。｀ESC[｀这个起始符加上约定的图像渲染码（m结尾），就work了，比如｀ESC［30m｀代表将字体的颜色设为黑色，参考[ANSI_escape_code](http://en.wikipedia.org/wiki/ANSI_escape_code#Colors)。多个图像渲染码可以用`;`分隔。

至于为什么这样写就能在各种console，各种shell中都能正确渲染，就是标准制定的功劳了

再回过头来看colors的代码，在输出每一个带格式的字符或字符串之后，后面都会跟一串reset渲染码。这里贴一下里面的代码，数组中第一个代表设置的值，第二个代表重置的值。
｀｀｀js
var codes = {
  reset: [0, 0],

  bold: [1, 22],
  dim: [2, 22],
  italic: [3, 23],
  underline: [4, 24],
  inverse: [7, 27],
  hidden: [8, 28],
  strikethrough: [9, 29],

  black: [30, 39],
  red: [31, 39],
  green: [32, 39],
  yellow: [33, 39],
  blue: [34, 39],
  magenta: [35, 39],
  cyan: [36, 39],
  white: [37, 39],
  gray: [90, 39],
  grey: [90, 39],

  bgBlack: [40, 49],
  bgRed: [41, 49],
  bgGreen: [42, 49],
  bgYellow: [43, 49],
  bgBlue: [44, 49],
  bgMagenta: [45, 49],
  bgCyan: [46, 49],
  bgWhite: [47, 49],

  // legacy styles for colors pre v1.0.0
  blackBG: [40, 49],
  redBG: [41, 49],
  greenBG: [42, 49],
  yellowBG: [43, 49],
  blueBG: [44, 49],
  magentaBG: [45, 49],
  cyanBG: [46, 49],
  whiteBG: [47, 49]

};
```

比如 console.log('\u001b[41m红色背景\u001b[49m又重置回白色背景了');

自行在node命令行里面试一下吧~