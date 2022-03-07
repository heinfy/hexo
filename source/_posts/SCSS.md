---
title: SASS
categories:
  - CSS
tags:
  - SCSS
abbrlink: ea50967e
date: 2020-03-24 08:19:20
---

<!-- more -->

# SASS

## 什么是 SASS

​SASS 是一种基于 CSS 的预处理语言，他在 CSS 的语法的基础上增加了变量(variables)\嵌套(nested rules)、混合(mixins)、导入(inline import)等高级功能，这样在开发项目是便于管理整体项目的样式风格。

### SASS 的特征

- 完全兼容 CSS3
- 在 CSS 基础上增加变量、嵌套 (nesting)、混合 (mixins) 等功能
- 通过函数进行颜色值与属性值的运算
- 提供控制指令等高级功能
- 自定义输出格式

### SASS 的语法格式

SASS 分为两种语法，一种是 SCSS，另一种就是 SASS。

SCSS 是最新的语法：

- 扩展名为.scss
- SCSS 语法规则和 CSS 的语法规则基本一样，使用大括号{}和分号;来分隔块的样式
- 增强了对 CSS3 语法的支持

SASS 是老语法：

- 扩展名为.sass
- SASS 语法也称之为 SASS 的缩进语法，使用缩进和换行来分块，而不是使用分号来分隔语句，和 stylus 这种预处理语言类似

**现在只学习 scss 语法**

##SCSS 语法

### 变量（Variables）

```scss
$color: #333; // 声明变量
$bgcolor: #f36;
body {
  color: $color; // 引用变量
  background-color: $bgcolor;
}
```

#### 变量默认值!default

```scss
$color: red;
$color: blue !default;
p {
  color: $color; //red
}
// 假设变量申明带有!default，如果在此申明之前没有这个变量的申明，则用这个值，
// 反之如果之前有申明，则用申明的值。
// 当然如果你先!default申明，然后再申明一次，那就没什么意思了，这就是基本的变量覆盖，第一次申明的有无!default都一样。
```

#### 变量用`#{}`包裹

```scss
$btnClass: btn !default;
$borderDirection: top !default;

.#{$btnClass} {
  border-#{$borderDirection}: 1px solid #ccc;
}

// 解析成的css：
.btn {
  border-top: 1px solid #ccc;
}
```

#### 多个变量一起申明

把多个相关的值写在一个变量里，然后通过`nth($var,index)`来获取第几个值。

```scss
$linkColor: red blue !default;
a {
  color: nth($linkColor, 1);
  &:hover {
    color: nth($linkColor, 2);
  }
}

// 解析成的css：
a {
  color: red;
}
a:hover {
  color: blue;
}
```

### 嵌套(Nesting)

SASS 中的嵌套有两种，一种是选择器的嵌套，另外一种是样式的嵌套。

然而这两种嵌套的目的都是一样的，减少代码量，增强代码的可读性。

#### 选择器的嵌套

```scss
$color: #333;
$bgcolor: #f36;

section {
  margin: 10px;
  background-color: #f36;
  nav {
    color: $color;
    height: 25px;
    a {
      color: #0982c1;
      &:hover {
        text-decoration: underline;
      }
    }
  }
}
```

#### 样式的嵌套

```scss
li {
  font: {
    style: italic;
    family: serif;
    weight: bold;
    size: 1.2em;
  }
}
// 当然font属性可以使用css3的合写，那就更简单了
li {
  font: serif italic bold 1.2em;
}
```

### Mixins

Mixins 是 SASS 中最强大的特性之一，简单点来说，Mixins 可以将一部分样式抽出，作为单独定义的模块，被很多选择器重复使用。

```scss
@mixin error($borderWidth: 2px) {
  border: $borderWidth solid #f00;
  color: #f00;
}

span {
  @include error(10px);
}
```

举例：

```scss
//Example: @include prefixer(border-radius, $radius, webkit spec);
//----------------------------------------
$prefix-for-webkit: true !default;
$prefix-for-mozilla: true !default;
$prefix-for-microsoft: true !default;
$prefix-for-opera: true !default;
$prefix-for-spec: true !default; // required for keyframe mixin
//prefixer
@mixin prefixer($property, $value, $prefixes) {
  @each $prefix in $prefixes {
    @if $prefix == webkit and $prefix-for-webkit == true {
      -webkit-#{$property}: $value !important;
    } @else if $prefix == moz and $prefix-for-mozilla == true {
      -moz-#{$property}: $value;
    } @else if $prefix == ms and $prefix-for-microsoft == true {
      -ms-#{$property}: $value;
    } @else if $prefix == o and $prefix-for-opera == true {
      -o-#{$property}: $value;
    } @else if $prefix == spec and $prefix-for-spec == true {
      #{$property}: $value;
    } @else {
      @warn "Unrecognized prefix: #{$prefix}";
    }
  }
}

@mixin skewX($degrees) {
  @include prefixer(transform, skewX($degrees), webkit moz o ms spec);
  -webkit-backface-visibility: hidden;
}
p {
  @include skewX(45deg);
}
```

编译后：

```css
p {
  -webkit-transform: skewX(45deg) !important;
  -moz-transform: skewX(45deg);
  -o-transform: skewX(45deg);
  -ms-transform: skewX(45deg);
  transform: skewX(45deg);
  -webkit-backface-visibility: hidden;
}
```

### @extend

假设现在要设计一个普通错误样式与一个严重错误样式，一般会这样写：

```html
<style>
  .error {
    border: 1px #f00;
    background-color: #fdd;
  }
  .seriousError {
    border-width: 3px;
  }
</style>
<div class="error seriousError">Oh no! You've been hacked!</div>
```

使用`@extend`：

```scss
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

**注意@extendh 会有一个缺点：**

```scss
.error {
  border: 1px #f00;
  background-color: #fdd;
}
// 这是一个其他样式
p .error {
  color: blue;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

但是在编译后的 CSS：

```css
.error,
.seriousError {
  border: 1px #f00;
  background-color: #fdd;
}

p .error,
p .seriousError {
  color: blue;
}

.seriousError {
  border-width: 3px;
}

// 然而你只想实现
.error,
.seriousError {
  border: 1px #f00;
  background-color: #fdd;
}

p .error {
  // 这部分的样式不能干扰到 seriousError
  color: blue;
}

.seriousError {
  border-width: 3px;
}
```

所以，通过`@extend`引用的类名，你要有绝对的自信，它从未用在几个地方。

### 嵌套、Mixins 和@extend 的缺点

嵌套：有可能编译的 css 层级过深

Mixins：Mixins 用来指定具体属性值会使 css 变得臃肿

**Mixins 的黄金规则**是将相似的风格定义在一个`@mixin`中。请注意这里的一个关键词**相似的**，另外 Mixins 主要是用于重用，而不是用来指定具体的属性值。

@extend：`@extend`是可以读取 SASS 文件中类名，这样就会出现意想不到的问题，如上的`.error`

### 强大的%placeholders

使用`%`和`@extend`就可以将继承中埋下的地雷给排了。

`%`只是一个占位符，他不是正常的选择器，不像`.classes`或者`#ids`，只要不通过`@extend`调用，他是不会产生任何代码量。

首先使用`%placeholders`定义一个公用样式，类似于`.class`：

```scss
%placeholders {
  color: red;
}
```

在需要使用的地方通过`@extend`来调用：

```scss
selector {
  @extend %placeholders;
}
```

举例：

```scss
@mixin fit-content() {
  width: -webkit-fit-content;
  width: -moz-fit-content;
  width: -o-fit-content;
  width: -ms-fit-content;
  width: fit-content;
}

%clearfix {
  *zoom: 1;
  &:after,
  &:before {
    content: '';
    display: table;
  }
  &:after {
    clear: both;
    overflow: hidden;
  }
}

nav {
  display: block;
  ul {
    margin: 50px auto;
    width: 800px;
    @include fit-content();
    padding: 0;
    list-style: none;
    @extend %clearfix;
  }
}
```

### @function

@function 与@mixin，%这两者的第一点不同在于 sass 本身就有一些内置的函数，方便我们调用，如强大的 color 函数；其次就是它返回的是一个值，而不是一段 css 样式代码什么的。

```scss
$f00: #f00;

//作为变量值
$redDark: darken($f00, 10%) !default;

//作为属性值
p {
  color: darken($f00, 70%);
  background-color: $redDark;
}
```

#### rgba

分为两种：`rgba($red, $green, $blue, $alpha)`和`rgba($color, $alpha)`。

第一种跟 css3 一样，不介绍，第二种对我们有点用，实例：

```scss
rgba(#102030, 0.5) => rgba(16, 32, 48, 0.5)
rgba(blue, 0.2)    => rgba(0, 0, 255, 0.2)
```

#### ie-hex-str

ie-hex-str($color)

这个函数将一个颜色格式化成 ie 滤镜使用，在做 css3 使用滤镜兼容的时候用得上，实例：

```scss
ie-hex-str(#abc) => #FFAABBCC
ie-hex-str(#3322BB) => #FF3322BB
ie-hex-str(rgba(0, 255, 0, 0.5)) => #8000FF00
```

#### darken

darken(color,color,amount)

第一个参数是颜色，第二参数是百分数介于 0%-100%，表示将某个颜色变暗多少个百分比。

```scss
darken(hsl(25, 100%, 80%), 30%) => hsl(25, 100%, 50%)
darken(#800, 20%) => #200
```

#### lighten

lighten(color,color,amount)

第一个参数是颜色，第二参数是百分数介于 0%-100%，表示将某个颜色变亮多少个百分比。

```scss
lighten(hsl(0, 0%, 0%), 30%) => hsl(0, 0, 30)
lighten(#800, 20%) => #e00
```

#### percentage

percentage($value)： 将一个没有单位的数字转成百分比形式

```scss
percentage(0.2) => 20%
percentage(100px / 50px) => 200%
```

#### length

length($list)

返回一个列表的长度

```scss
length(10px) => 1
length(#514721 #FFF6BF #FFD324) => 3
```

#### nth

nth(list,list,n);

返回列表里面第 n 个位置的值

```scss
nth(10px 20px 30px, 1) => 10px
nth((Helvetica, Arial, sans-serif), 3) => sans-serif
```

#### unit

unit($number)

得到这个数的单位

```scss
unit(100) => ""
unit(100px) => "px"
unit(3em) => "em"
unit(10px * 5em) => "em*px"
unit(10px * 5em / 30cm / 1rem) => "em*px/cm*rem"
```

#### unitless

unitless($number)

返回这个数是否没有单位

```scss
unitless(100) => true
unitless(100px) => false
```

#### 三目判断

if(condition,condition,if-true, $if-false)

第一个表示条件，第二个表示条件为真的值，第三个表示为假的值

```scss
if(true, 1px, 2px) => 1px
if(false, 1px, 2px) => 2px
```

注意函数是返回一个值，不能直接放到 sass 里面直接去运行的，会报错。

你可以把这些用在判断或者属性值里面，那么就是一级棒。

下面我们来搞点自己定义的函数吧，先来个简单的：

```scss
// px转em
@function pxToEm($px, $base: 16) {
  @return ($px / $base) * 1em;
}
// 调用下：
p {
  font-size: pxToEm(20);
}
// 解析后的css：
p {
  font-size: 1.25em;
}
```

### @if 和@else

```scss
$filter: false !default; //是否开启ie滤镜
//背景色半透明
@mixin bgcolor-alpha($bgcolor: rgba(0, 0, 0, 0.5)) {
  color: #fff;
  @if $filter {
    filter: progid:DXImageTransform.Microsoft.gradient(enabled='true',startColorstr='#{ie-hex-str($bgcolor)}', endColorstr='#{ie-hex-str($bgcolor)}');
  } @else {
    background-color: #333;
  }
  background-color: $bgcolor;
}
```

这是半透明 rgba 背景的一段代码，高级浏览器用 rgba，ie6-8 如果开启滤镜用滤镜，不开启滤镜就用纯色，常用于图片下方浮现标题。

当然也不可能总是判断一个变量的真假那么简单，没有或与非的情况吧。sass 的@if 用`not,or,and`分别表示非，或，与。

```scss
$a: false !default;
$b: true !default;

@if not($a) {
  p {
    color: red;
  }
}

div {
  font-size: 14px;
  @if $a or $b {
    width: 300px;
  }
}

li {
  line-height: 24px;
  @if $a and $b {
    float: left;
  }
}
```

想想还是漏了个，sass 用`==,!=`分别表示等于与不等于。

```scss
$radius: 5px !default;

.box-border {
  border: 1px solid #ccc;
  padding: 10px;

  @if $radius != 0 {
    border-radius: $radius;
  }
}

$sizeClass: small !default;

.size {
  @if $sizeClass == 'small' {
    padding: 5px;
  } @else {
    padding: 10px;
  }
}
```

### @for

for 循环有两种形式，分别为：``@for $var from start through end` 和`@for $var start from to end` 。$i 表示变量，start 表示起始值，end 表示结束值，这两个的区别是关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数。先来个简单的：

```scss
@for $i from 1 through 3 {
  .item-#{$i} {
    width: 2em * $i;
  }
}
```

### @each

语法：@each $var in

```scss
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
```

```scss
$sprite: puma sea-slug egret salamander !default;

%sprite-animal {
  background: url('/images/animal.png') no-repeat;
}
@each $animal in $sprite {
  .#{$animal}-icon {
    @extend %sprite-animal;
    background-position: 0 - (index($sprite, $animal) * 30px);
  }
}
```

使用 scss 设计按钮样式：

```scss
//$btnColorClass: (primary #0078E7 #fff) (green $green #fff);
$btnColorClass: (primary #0078e7 #fff) (blue #00a3cf #fff) (orange #f47837 #fff) !default;

@mixin btn-color($bgColor: #e6e6e6, $textColor: #333) {
  color: $textColor;
  background-color: $bgColor;
  border: 1px solid darken($bgColor, 5%);
  border-color: lighten($bgColor, 2%) darken($bgColor, 5%) darken($bgColor, 5%) lighten($bgColor, 2%);

  &:hover {
    background-color: darken($bgColor, 5%);
    color: $textColor;
  }
}

@each $colorClass in $btnColorClass {
  $class: nth($colorClass, 1);
  $bgColor: nth($colorClass, 2);
  $textColor: nth($colorClass, 3);
  .btn-#{$class} {
    @include btn-color($bgColor, $textColor);
  }
}
```

## @include vs @extend

避免代码重复生成，

SASS 中产生了 Mixins，我们可以将相似的样式定义成一个函数模块，然后通过`@include`来调用。但很多时候，我们又不需要这么强大的功能。

这个时候出现`@extend`来调用定义好相同样式的类，可没想到，这个功能是方便了，但无形中为使用者埋下了一个地雷。

为了解除这个隐患，在 SASS3.2 中增加了一个`%placeholders`功能。让大家能很方便定义一些功能简单的相同样式模块。

通过前面的介绍`@mixin`需要`@include`来调用，而`.class`和`%placeholders`需要`@extend`来调用，那么两者有何区别呢？

- `@include`主要是用来调用`@mixin`定义的函数模块。在`@mixin`中可以定义一个相似功能样式，而且可以设置变量、定义参数和默认参数值；
- `@extend`主要是用来调用`.class`或者`%placeholders`定义的属性模块；在`.class`或者`%placeholders`中可以定义一个相同样式，但这里面不能定义参数；
- `@include`每次调用相同的`@mixin`时，编译出来的 CSS 相同样式不会进行合并；
- `@extend`每次调用相同的 `.class`时，如果`.class`在样式出现多次，那么编译出来的 CSS 有可能不是需要的样式；
- `@extend`每次调用相同的`%placeholders`时，编译出来的 CSS 相同样式会进行合并。

下面我们通过一个清除浮动的案例，分别看看`@include`和`@extend`之间的区别：

### `@include`与`@mixin`使用例子

```scss
@mixin clearfix {
  & {
    *zoom: 1;
  }
  &:before,
  &:after {
    display: table;
    content: '';
  }
  &:after {
    clear: both;
    overflow: hidden;
  }
}

ul {
  @include clearfix;
}
.block {
  @include clearfix;
}
```

编译后：

```css
ul {
  *zoom: 1;
}
ul:before,
ul:after {
  display: table;
  content: '';
}
ul:after {
  clear: both;
  overflow: hidden;
}

.block {
  *zoom: 1;
}
.block:before,
.block:after {
  display: table;
  content: '';
}
.block:after {
  clear: both;
  overflow: hidden;
}
```

很明显，相同的样式不会进行合并。

### `@include`和`%placeholders`使用例子

```scss
%clearfix {
  & {
    *zoom: 1;
  }
  &:before,
  &:after {
    display: table;
    content: '';
  }
  &:after {
    clear: both;
    overflow: hidden;
  }
}

ul {
  @extend %clearfix;
}
.block {
  @extend %clearfix;
}
```

编译后：

```css
.block,
ul {
  *zoom: 1;
}
.block:before,
ul:before,
.block:after,
ul:after {
  display: table;
  content: '';
}
.block:after,
ul:after {
  clear: both;
  overflow: hidden;
}
```

很明显相同样式代码已经进行合并。

### Mixins 与%placeholders 的结合

Mixins 如果使用不当，就会产生很多重复的代码，但仅用`@extend`很多时候又无法达到功能上的需求。

那么有没有方法能把 Mixins 与`%placeholders`结合起来，取他们各自的优势呢？

大家都知道,`%placeholders`就类似于 CSS 中的`.classes`或者`#ids`，只不过使用`%`代替了`.`和`#`。但`%placeholders`中的代码只有通过 `@extend`调用之后才会产生代码量，不然他是不会产生任何代码量。

下面我们来看一个 Mixins 与`%placeholders`结合在一起制作的一个网格系统。

```scss
%grid {
  box-sizing: border-box;
  display: inline-block;
  padding-left: 1em;
  padding-right: 1em;
}

@mixin grid($width: 1) {
  @extend %grid;
  width: percentage($width);
}

.grid-half {
  @include grid(1 / 2);
}

.grid-third {
  @include grid(1 / 3);
}
```

按照这样的方法，我们可以制作出一个简单的百分比网格系统：

```scss
$columns: 12;
$gutter: 2em;

%grid {
  box-sizing: border-box;
  display: inline-block;
  padding: {
    left: $gutter / 2;
    right: $gutter / 2;
  }
}

@mixin grid($width: 1) {
  @extend %grid;
  width: percentage($width);
}

@for $column from 1 through $columns {
  .grid-#{$column} {
    @include grid(1 / $column);
  }
}
```

## 使用 Mixins 和继承的细节

了解了`@include`定义的`@mixin`,`@extend`定义的`.class`和`@extend`定义的`%placeholders`差异之后，我们在写 SASS 时，有一些细节大家应该了解：

- 不要使用没有设置参数的`@mixin`，他们应该是`.class`或者`%placeholders`;
- 不要轻意（从不使用）`@extend`调用`.class`。会得到你意想不到的结果，特别是定义的`.class`出现在嵌套或其他的样式表中，你应该使用`@extend`调用`%placeholders`;
- 不要使用太深的选择器嵌套。
- 如果你能避免，不要使用标签名。这不是一个 taxative 规则，但比 id 或者类名的性能要更低；
- 不要使用子选择器符号`>`，在 SASS 中很无效；
- 不要使用同史选择器`+`，配合你当前的标记他是非常无效。
- 不要太相信 SASS 的自动编译，你应该时时检查生成的 CSS。在 SASS 中纠错能力比较差；

## 案例实战

![案例实战](SCSS/%E6%A1%88%E4%BE%8B%E5%AE%9E%E6%88%98.gif)

1.`html`结构

```html
<ul class="menu">
  <li><a href="">首页</a></li>
  <li>
    <a href="">博客</a>
    <ul class="drop-menu">
      <li><a href="">CSS3</a></li>
      <li><a href="">SASS</a></li>
      <li><a href="">JavaScript</a></li>
      <li><a href="">jQuery</a></li>
    </ul>
  </li>
  <li><a href="">案例</a></li>
  <li><a href="">资源</a></li>
  <li><a href="">前端收藏夹</a></li>
</ul>
```

有了结构，我们就要开始动手了，在动手之前对这个效果先简单的分析一下：

- **定义变量**：我要定义几个变量，方便换成别的风格；
- **清除浮动**：列表使用了浮动，需要清除浮动
- **清除列表默认样式**：导航是使用 ul 制作，所以需要清除其默认样式
- **定义 transfrom**：效果中使用到了 CSS3 的 transform，使用`@mixin`定义成一个模块
- **定义 transition**：下拉菜单出现的时候有一个 transition 效果
- **定义 fit-content**：使用 CSS3 的 fit-content
- **定义 box-shadow**：使用 CSS3 的 box-shadow
- **定义文本**：设置菜单项文本效果

1. 定义变量

```scss
//1.定义变量
$color: #fff !default; //设置文本颜色
$bgColor: #34495e !default; //设置背景色
$sfbgColor: #e74c3c !default; //设置悬浮背景色
$fontSize: 14px !default; //设置字号
$fontFamily: Arial, Helvetica !default; //设置字体
$width: 462px !default; //设置默认宽度
```

2. 清除浮动

```scss
//2.使用%placeholders定义清除浮动
%clearfix {
  &{
    *zoom: 1;
  }
  &:before,
  &:after{
    content: "";
    display: table;
  }
  &:after {
    clear: both;
    overflow: hidden;
  }
```

3. 清除列表默认样式

```scss
//3.清除列表默认样式
%listStyle {
  margin: 0;
  padding: 0;
  list-style: none outside none;
}

$prefix-for-webkit: true !default;
$prefix-for-mozilla: true !default;
$prefix-for-microsoft: true !default;
$prefix-for-opera: true !default;
$prefix-for-spec: true !default;
```

4. 定义 transfrom

```scss
// 4.浏览器前缀
@mixin prefixer($property, $value, $prefixes) {
  @each $prefix in $prefixes {
    @if $prefix == webkit and $prefix-for-webkit == true {
      -webkit-#{$property}: $value;
    } @else if $prefix == moz and $prefix-for-mozilla == true {
      -moz-#{$property}: $value;
    } @else if $prefix == ms and $prefix-for-microsoft == true {
      -ms-#{$property}: $value;
    } @else if $prefix == o and $prefix-for-opera == true {
      -o-#{$property}: $value;
    } @else if $prefix == spec and $prefix-for-spec == true {
      #{$property}: $value;
    } @else {
      @warn "Unrecognized prefix: #{$prefix}";
    }
  }
}
```

5. 定义 transition

```scss
//5.定义transform
//示例: @include prefixer(border-radius, $radius, webkit spec);
//Transform, transform-origin, transform-style
//----------------------------------------
@mixin transform($property...) {
  @include prefixer(transform, $property, webkit moz o ms spec);
}

@mixin transform-origin($axes: 50%) {
  // x-axis - left | center | right  | length | %
  // y-axis - top  | center | bottom | length | %
  // z-axis - length
  @include prefixer(transform-origin, $axes, webkit moz o ms spec);
}

@mixin skewX($degrees) {
  @include prefixer(transform, skewX($degrees), webkit moz o ms spec);
  -webkit-backface-visibility: hidden;
}
```

6. 定义 fit-content

```scss
//6.定义transition
// Return vendor-prefixed property names if appropriate
// Example: transition-property-names((transform, color, background), moz) -> -moz-transform, color, background//----------------------------------------
@function transition-property-names($props, $vendor: false) {
  $new-props: ();
  @each $prop in $props {
    $new-props: append($new-props, transition-property-name($prop, $vendor), comma);
  }
  @return $new-props;
}
@function transition-property-name($prop, $vendor: false) {
  // put other properties that need to be prefixed here aswell
  @if $vendor and $prop == transform {
    @return unquote('-' + $vendor + '-' + $prop);
  } @else {
    @return $prop;
  }
}
// transition
//----------------------------------------
@mixin transition($properties...) {
  @if length($properties) >= 1 {
    @include prefixer(transition, $properties, webkit moz o ms spec);
  } @else {
    $properties: all 0.15s ease-out 0;
    @include prefixer(transition, $properties, webkit moz o ms spec);
  }
}
```

7. 定义 fit-content

```scss
//7.定义fit-content
@mixin fit-content {
  width: -webkit-fit-content;
  width: -moz-fit-content;
  width: -o-fit-content;
  width: -ms-fit-content;
  width: fit-content;
}
```

8. 定义 box-shadow

```scss
//8.设置box-shadow
// box-shadow
@mixin box-shadow($shadow...) {
  @include prefixer(box-shadow, $shadow, webkit spec);
}
```

9. 定义文本

```scss
//9.设置文本
%typography {
  color: $color;
  text: {
    decoration: none;
    align: center;
  }
  font: {
    family: $fontFamily;
    size: $fontSize;
  }
}
```

10. 具体样式

```scss
// 10.具体样式
.menu {
  width: $width;
  @extend %clearfix; //调用清除浮动
  @extend %listStyle; //调用清除列表样式
  @include fit-content;
  margin: 50px auto;
}

.drop-menu {
  @extend %listStyle; //调用清除列表样式
}

.menu > li {
  background: $bgColor;
  float: left;
  position: relative;
  @include skewX(25deg);
}

.menu a {
  display: block;
  @extend %typography;
}

.menu li:hover {
  background: $sfbgColor;
}

.menu > li > a {
  padding: 1em 2em;
  @include skewX(-25deg);
}

/*Dropdown menu*/
.drop-menu {
  position: absolute;
  width: $width / 4;
  left: 50%;
  margin-left: -($width / 8);
  opacity: 0;
  visibility: hidden;
  @include skewX(-25deg);
  @include transform-origin(left top);
  li {
    background-color: $bgColor;
    position: relative;
    overflow: hidden;
    opacity: 0;
    visibility: hidden;
    @include transition(all 0.2s ease);
    a {
      padding: 1em 2em;
    }
    &::after {
      content: '';
      position: absolute;
      top: -125%;
      height: 100%;
      width: 100%;
      @include box-shadow(0 0 50px rgba(0, 0, 0, 0.9));
    }
    &:nth-child(odd) {
      @include transform(skewX(-25deg) translateX(0));
      a {
        @include skewX(25deg);
      }
      &::after {
        right: -50%;
        @include transform(skewX(-25deg) rotate(3deg));
      }
    }
    &:nth-child(even) {
      @include transform(skewX(25deg) translateX(0));
      a {
        @include skewX(-25deg);
      }
      &::after {
        left: -50%;
        @include transform(skewX(25deg) rotate(3deg));
      }
    }
  }
}

.menu > li:hover .drop-menu,
.menu > li:hover .drop-menu li {
  opacity: 1;
  visibility: visible;
}

.menu > li:hover .drop-menu li:nth-child(even) {
  @include transform(skewX(25deg) translateX(15px));
}

.menu > li:hover .drop-menu li:nth-child(odd) {
  @include transform(skewX(-25deg) translateX(-15px));
}
```

## 阅读

- [Sass 中文网](https://www.sass.hk/)
- [SASS 基础教程——SASS 基本语法与特性](https://www.w3cplus.com/preprocessor/sass-basic-syntax-and-features.html)
- [sass 揭秘之变量](https://www.w3cplus.com/preprocessor/sass-basic-variable.html)
- [理解 SASS 的嵌套，@extend，%Placeholders 和 Mixins](https://www.w3cplus.com/preprocessor/sass-basic-mixins-nesting-placeholders-extend.html)
- 在线编辑 SCSS 工具：[sassmeister](https://www.sassmeister.com/)
- [sass 揭秘之@if，@for，@each（转载）](https://www.cnblogs.com/dingyufenglian/p/4864392.html)
