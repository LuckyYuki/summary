使用其它的JavaScript库
要想描述非TypeScript编写的类库的类型，我们需要声明类库所暴露出的API。

我们叫它声明因为它不是“外部程序”的具体实现。 它们通常是在 .d.ts文件里定义的。 如果你熟悉C/C++，你可以把它们当做 .h文件。 让我们看一些例子。

外部模块
在Node.js里大部分工作是通过加载一个或多个模块实现的。 我们可以使用顶级的 export声明来为每个模块都定义一个.d.ts文件，但最好还是写在一个大的.d.ts文件里。 我们使用与构造一个外部命名空间相似的方法，但是这里使用 module关键字并且把名字用引号括起来，方便之后import。 例如：

node.d.ts (simplified excerpt)
declare module "url" {
    export interface Url {
        protocol?: string;
        hostname?: string;
        pathname?: string;
    }

    export function parse(urlStr: string, parseQueryString?, slashesDenoteHost?): Url;
}

declare module "path" {
    export function normalize(p: string): string;
    export function join(...paths: any[]): string;
    export let sep: string;
}
现在我们可以/// <reference> node.d.ts并且使用import url = require("url");或import * as URL from "url"加载模块。

/// <reference path="node.d.ts"/>
import * as URL from "url";
let myUrl = URL.parse("http://www.typescriptlang.org");
外部模块简写
假如你不想在使用一个新模块之前花时间去编写声明，你可以采用声明的简写形式以便能够快速使用它。

declarations.d.ts
declare module "hot-new-module";
简写模块里所有导出的类型将是any。

import x, {y} from "hot-new-module";
x(y);
模块声明通配符
某些模块加载器如SystemJS 和 AMD支持导入非JavaScript内容。 它们通常会使用一个前缀或后缀来表示特殊的加载语法。 模块声明通配符可以用来表示这些情况。

declare module "*!text" {
    const content: string;
    export default content;
}
// Some do it the other way around.
declare module "json!*" {
    const value: any;
    export default value;
}
现在你可以就导入匹配"*!text"或"json!*"的内容了。

import fileContent from "./xyz.txt!text";
import data from "json!http://example.com/data.json";
console.log(data, fileContent);
UMD模块
有些模块被设计成兼容多个模块加载器，或者不使用模块加载器（全局变量）。 它们以 UMD模块为代表。 这些库可以通过导入的形式或全局变量的形式访问。 例如：

math-lib.d.ts
export function isPrime(x: number): boolean;
export as namespace mathLib;
之后，这个库可以在某个模块里通过导入来使用：

import { isPrime } from "math-lib";
isPrime(2);
mathLib.isPrime(2); // 错误: 不能在模块内使用全局定义。
它同样可以通过全局变量的形式使用，但只能在某个脚本（指不带有模块导入或导出的脚本文件）里。

mathLib.isPrime(2);
