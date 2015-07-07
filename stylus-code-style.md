# Stylus 编码规范 (Draft)

## 简介

该文档主要的设计目标是提高 Stylus 文档的团队一致性与可维护性。

Stylus 代码的基本规范和原则与 [CSS 编码规范](http://styleguide.baidu.com/style/css/index.html) 保持一致。

## 要求

在本文档中，使用的关键字会以中文+括号包含的关键字英文表示：_必须 (MUST)_。关键字"MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL"被定义在rfc2119中。

## 代码组织

(待整理...)

## 文件

* Stylus 文件 _推荐 (RECOMMENDED)_ 使用 `UTF-8` 编码。
* 使用 `UTF-8` 编码时，文件 _不得 (MUST NOT)_ 包含 `BOM` 信息。

> UTF-8 编码具有更广泛的适应性。BOM 在使用程序或工具处理文件时可能造成不必要的干扰。

## 缩进

* _必须 (MUST)_ 使用 `4` 个空格做为一个缩进层级，_不得 (MUST NOT)_ 使用 `2` 个空格或 `tab` 字符。

> 采用 4 空格缩进，层级关系的辨识度更高，在各种环境下的表现更统一。

```stylus
// good
.selector
    color: #333

// bad
.selector
  color: #333
```

## 冒号

* `属性名` 与 `属性值` 之间 _必须 (MUST)_ 用 `:` 分隔。

> 采用冒号分隔，增强属性名与属性值之前的辨识度。

```stylus
// good
.selector
    margin: 10px

// bad
.selector
    margin 10px
```

## 分号

* 属性定义后，结尾 _不得 (MUST NOT)_ 出现 `;`。

> 属性的定义必须独占一行，所以行尾的分号是不必要的。

```stylus
// good
.selector
    margin: 0

// bad
.selector
    margin: 0;
```

## 大括号

* `选择器` 对应的 `声明块` _不得 (MUST NOT)_ 使用 `{}` 进行包裹。

> 层次关系通过缩进表达，所以声明块周围的大括号是不必要的。

```stylus
// good
.selector
    margin: 0

// bad
.selector {
    margin: 0
}
```

## 空格

* `属性名` 与之后的 `:` 之间 _不得 (MUST NOT)_ 包含空格，`:` 与 `属性值` 之间 _必须 (MUST)_ 包含空格。
* `列表型属性值` 书写在单行时，`,` 后 _必须 (MUST)_ 跟一个空格。

```stylus
// good
.selector
    font-family: Arial, sans-serif

// bad
.selector
    font-family:Arial,sans-serif
```

## 引号

* 引号 _可以 (MAY)_ 使用 `'` 或 `"`，但在同一项目中 _必须 (MUST)_ 统一。

```stylus
// good
@require 'normalize'
@require 'logic/mixins'

// bad
@require 'normalize'
@require "logic/mixins"
```

## 依赖与文件引入

* 需要引入文件时，_建议 (RECOMMENDED)_ 优先使用 `@require`，只在需要重复输出 CSS 时使用 `@import`。
* 如果依赖其他文件提供的变量、Mixin、Function 等功能，且没有 _隐式注入_ 依赖关系时，_必须 (MUST)_ 显式声明依赖关系。

> 隐式注入多见于各类样式库，其原理是通过 JavaScript API 预先导入依赖。

```stylus
// 显式声明依赖，需要用到来自 `common/icon` 的 set-icon 方法
@require 'common/icon'

.back
    &:before
        set-icon: back
```

## 变量

* 变量 _必须 (MUST)_ 以 `$` 为前缀，字母全部小写，多个单词以 `-` 链接。

```stylus
// good
$box-color = #333

// bad
box-color = #333
$box_color = #333
$BoxColor = #333
$boxColor = #333
$BOX_COLOR = #333
```

* 跨文件调用的全局变量 _必须 (MUST)_ 以 `$-` 为前缀，字母全部小写，多个单词以 `-` 链接。

```stylus
// a.styl
$-base-font-size = 16px

// b.styl
@require 'a'

body
    font-size: $-base-font-size
```

## Mixin

* Mixin 的命名 _必须 (MUST)_ 字母全小写，多个单词以 `-` 链接。

```stylus
// good
triangle()
set-icon()

// bad
$set-icon()
setIcon()
SetIcon()
set_icon()
```

* Mixin 的调用方式有 `mixin-name: args...` 与 `mixin-name(args...)` 两种风格，_建议 (RECOMMENDED)_：
    * 无参数传入的 Mixin 以 `mixin-name(args...)` 方式调用。
    * 在 `根节点` 使用的 Mixin 以 `mixin-name(args...)` 方式调用。
    * 其余 Mixin 以 `mixin-name: args...` 方式调用。

```stylus
// good
font-face('Open Sans', '../font/open-sans')
.selector
    size: 300px 88px
    clearfix()

// bad
font-face: 'Open Sans', '../font/open-sans'
.selector
    size(300px 88px)
```

* 如果 Mixin 通过覆盖现有 CSS `属性名` 拓展能力，_必须 (MUST)_ 保证原 `属性名` 的原功能不发生变化。

## Block Mixin

(待整理...)

## Function

* Function 的命名 _必须 (MUST)_ 字母全小写，多个单词以 `-` 链接（同 `Mixin`）。

## 继承

* 继承时，_必须 (MUST)_ 使用 `@extend` 语句，_不得 (MUST NOT)_ 使用 `@extends`。
* 继承时，`@extend` 语句 _必须 (MUST)_ 写在 `声明块` 的开头。

```stylus
// good
.sub
    @extend $mod
    color: red

// bad
.sub
    @extends $mod
    color: red

// bad
.sub
    color: red
    @extend $mod
```
