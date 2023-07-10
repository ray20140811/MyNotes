# [JavaScript的CJS、AMD、UMD、ESM都是啥](https://segmentfault.com/a/1190000040282826)

在最开始JavaScript没有import / export模块这些机制。所有的代码都在一个文件里，这简直就是灾
难。

之后就出现了一些机制改变只有一个文件的问题。于是就出现了CJS、AMD、UMD和ESM。这篇小文就
是让大家了解这些都是什么。

CJS

CJS全称CommonJS。看起来是这样的：

//importing 
const doSomething = require('./doSomething.js'); 

//exporting
module.exports = function doSomething(n) {
  // do something
}

CJS经常在node开发中出现。

* CJS使用同步方式引入模块
* 你可以从node_modules或者本地目录引入模块。如：const someModule = require
 ('./some/local/file');。
* CJS引入模块的一个复制文件。
* CJS不能在浏览器里工作。要在浏览器里使用，则需要转码和打包

AMD

AMD的全称是异步模块定义。如：

define(['dep1', 'dep2'], function (dep1, dep2) {
    //Define the module value by returning a value.
    return function () {};
});

或者

// "simplified CommonJS wrapping" https://requirejs.org/docs/whyamd.html
define(function (require) {
    var dep1 = require('dep1'),
        dep2 = require('dep2');
    return function () {};
});

* AMD异步引入模块，也是因此得名
* ADM是给前端用的
* AMD不如CJS直观

UMD

UMD全称是Universal Module Definition。如：

(function (root, factory) {
    if (typeof define === "function" && define.amd) {
        define(["jquery", "underscore"], factory);
    } else if (typeof exports === "object") {
        module.exports = factory(require("jquery"), require("underscore"));
    } else {
        root.Requester = factory(root.$, root._);
    }
}(this, function ($, _) {
    // this is where I defined my module implementation

    var Requester = { // ... };

    return Requester;
}));

* 可以在前端和后端通用
* 与CJS和AMD不同，UMD更像是配置多模块系统的模式
* UMD通常是Rollup、webpack的候补选择

ESM

ESM的全称是ES Modules。如：

import React from 'react';

或者

import {foo, bar} from './myLib';

...

export default function() {
  // your Function
};
export const function1() {...};
export const function2() {...};

* 在很多现代的浏览器里都可以用
* 前后端都可以用。CJS一样的简单的语法规则和AMD异步摇树
* ESM允许打包工具，比如Rollup、webpack去除不必要的代码
* 在HTML调用

<script type="module">
import {func1} from 'my-lib';

func1();
</script>

暂时还没有得到全部浏览器的支持。

总结

* ESM得益于简单的语法、异步和摇树的特点，基本上就是最好的模块机制了
* UMD哪里都可以用，所以被用作备用打包方案
* CJS是同步的，在后端中用的比较多
* AMD是异步的，对前端友好

