# sass

###### 世界上最成熟、最稳定、最强大的专业级CSS扩展语言！

## 安装Sass和Compass

window下安装SASS首先需要安装`Ruby`，先从官网[下载Ruby](http://rubyinstaller.org/downloads)并安装。安装过程中请注意勾选`Add Ruby executables to your PATH`添加到系统环境变量

```
ruby -v
//如安装成功会打印
ruby 2.2.2p95 (2015-04-13 revision 50295) [i386-mingw32]
```

```
//1.删除原gem源
gem sources --remove https://rubygems.org/

//2.添加国内淘宝源,https://gems.ruby-china.com/
$ gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

//3.打印是否替换成功
gem sources -l

//4.更换成功后打印如下
*** CURRENT SOURCES ***
https://gems.ruby-china.com/
```

```
//安装如下(如mac安装遇到权限问题需加 sudo gem install sass)
gem install sass
gem install compass

//更新sass
gem update sass
//查看sass版本
sass -v
//查看sass帮助
sass -h

```

## 编译sass

`sass`编译有很多种方式，如命令行编译模式、sublime插件`SASS-Build`、编译软件`koala`、前端自动化软件`codekit`、Grunt打造前端自动化工作流`grunt-sass`、Gulp打造前端自动化工作流`gulp-ruby-sass`等。

## 命令行编译;

```
//单文件转换命令
sass input.scss output.css

//单文件监听命令
sass --watch input.scss:output.css

//如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
sass --watch app/sass:public/stylesheets
```

### 命令行编译配置选项;

命令行编译`sass`有配置选项，如编译过后css排版、生成调试map、开启debug信息等，
可通过使用命令`sass -v`查看详细。我们一般常用两种`--style``--sourcemap`。

```
//编译格式
sass --watch input.scss:output.css --style compact

//编译添加调试map
sass --watch input.scss:output.css --sourcemap

//选择编译格式并添加调试map
sass --watch input.scss:output.css --style expanded --sourcemap

//开启debug信息
sass --watch input.scss:output.css --debug-info
```

- `--style`表示解析后的`css`是什么排版格式;
   sass内置有四种编译格式:`nested``expanded``compact``compressed`。
- `--sourcemap`表示开启`sourcemap`调试。开启`sourcemap`调试后，会生成一个后缀名为`.css.map`文件。

### 四种编译排版演示;

```
nested  多拍缩进格式
expanded 多排格式
compact 每个一排，最喜欢的
compressed 压缩格式，最小
```

# 1. 使用变量

```
$nav-color: #F90;
nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}

```

# 2. 嵌套CSS 规则;

###### 2-1.父选择器的标识符&;

```
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }

#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
article a {
  color: blue;
  &:hover { color: red }  
}
```

###### 2-2.群组选择器的嵌套

```
.container h1, .container h2, .container h3 { margin-bottom: .8em }

.container {
  h1, h2, h3 {margin-bottom: .8em}
}
```

###### 2-3.子组合选择器和同层组合选择器：>、+和~;

```
article section { margin: 5px }  择article下的所有命中section
article > section { border: 1px solid #ccc }  紧跟着的子元素
header + p { font-size: 1.1em } 同层相邻组合选择器+选择header元素后紧跟的p元素
article ~ article { border-top: 1px dashed #ccc } ，选择所有跟在article后的同层article元素，不管它们之间隔了多少其他元素：
```

###### 2-4.嵌套属性

```
nav {  反复写border-styleborder-widthborder-color以及border-*等也是非常烦人的
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}

nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}

nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
```

# 3. 导入SASS文件;

使用`sass`的`@import`规则并不需要指明被导入文件的全名。你可以省略`.sass`或`.scss`文件后缀。**生成`css`文件时就把相关文件导入进来**

###### 3-1. 使用SASS部分文件;

```
你想导入themes/_night-sky.scss这个局部文件里的变量，你只需在样式表中写@import "themes/night-sky";。
```

###### 3-2. 默认变量值;

```
$link-color: blue;
$link-color: red;
a {
color: $link-color;
}

很像css属性中!important标签的对立面，不同的是!default用于变量，含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。

$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```

###### 3-3. 嵌套导入;

```
aside {
  background: blue;
  color: white;
}

.blue-theme {@import "blue-theme"}

//生成的结果跟你直接在.blue-theme选择器内写_blue-theme.scss文件的内容完全一样。

.blue-theme {
  aside {
    background: blue;
    color: #fff;
  }
}
```

# 3-4. 原生的CSS导入;

你不能用`sass`的`@import`直接导入一个原始的`css`文件，因为`sass`会认为你想用`css`原生的`@import`。但是，因为`sass`的语法完全兼容`css`，所以你可以把原始的`css`文件改名为`.scss`后缀，即可直接导入了。

# 4. 静默注释;

```
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

# 5. 混合器;

```
下边的这段sass代码，定义了一个非常简单的混合器，目的是添加跨浏览器的圆角边框。
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}

notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
//sass最终生成：
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

###### 5-2. 混合器中的CSS规则;

```
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}

ul.plain {
  color: #444;
  @include no-bullets;
}
最终，上边的例子如下代码:
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```

###### 5-3. 给混合器传参;

```
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
当混合器被@include时，你可以把它当作一个css函数来传参。如果你像下边这样写：
a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }

sass允许通过语法$name: value的形式指定每个参数的值，解决参数顺序问题
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```

###### 5-4. 默认参数值;

```
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
  
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

**混合器只是`sass`样式重用特性中的一个。我们已经了解到混合器主要用于样式展示层的重用，如果你想重用语义化的类呢？这就涉及`sass`的另一个重要的重用特性：选择器继承。**

# 6. 使用选择器继承来精简CSS;

使用`sass`的时候，最后一个减少重复的主要特性就是选择器继承。基于`Nicole Sullivan`面向对象的`css`的理念，选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过`@extend`语法实现，如下代码:

```
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

在上边的代码中，`.seriousError`将会继承样式表中任何位置处为`.error`定义的所有样式。以`class="seriousError"` 修饰的`html`元素最终的展示效果就好像是`class="seriousError error"`。相关元素不仅会拥有一个`3px`宽的边框，而且这个边框将变成红色的，这个元素同时还会有一个浅红色的背景，因为这些都是在`.error`里边定义的样式。

`.seriousError`不仅会继承`.error`自身的所有样式，任何跟`.error`有关的组合选择器样式也会被`.seriousError`以组合选择器的形式继承，如下代码:

```
//.seriousError从.error继承样式
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}
```

如上所示，在`class="seriousError"`的`html`元素内的超链接也会变成红色和粗体。

本节将介绍与混合器相比，哪种情况下更适合用继承。接下来在探索继承的工作细节之前，我们先了解一下继承的高级用法。最后，我们将看看使用继承可能会有哪些坑，学习如何避免这些坑。

# 6-1. 何时使用继承;

5-1节介绍了[混合器](https://www.sass.hk/guide/)主要用于展示性样式的重用，而类名用于语义化样式的重用。因为继承是基于类的（有时是基于其他类型的选择器），所以继承应该是建立在语义化的关系上。当一个元素拥有的类（比如说`.seriousError`）表明它属于另一个类（比如说`.error`），这时使用继承再合适不过了。

这有点抽象，所以我们从几个方面来阐释一下。想象一下你正在编写一个页面，给`html`元素添加类名，你发现你的某个类（比如说`.seriousError`）另一个类（比如说`.error`）的细化。你会怎么做？

- 你可以为这两个类分别写相同的样式，但是如果有大量的重复怎么办？使用`sass`时，我们提倡的就是不要做重复的工作。
- 你可以使用一个选择器组（比如说`.error``.seriousError`）给这两个选择器写相同的样式。如果.error的所有样式都在同一个地方，这种做法很好，但是如果是分散在样式表的不同地方呢？再这样做就困难多了。
- 

- 你可以使用一个混合器为这两个类提供相同的样式，但当`.error`的样式修饰遍布样式表中各处时，这种做法面临着跟使用选择器组一样的问题。这两个类也不是恰好有相同的 样式。你应该更清晰地表达这种关系。
- 综上所述你应该使用`@extend`。让`.seriousError`从`.error`继承样式，使两者之间的关系非常清晰。更重要的是无论你在样式表的哪里使用`.error``.seriousError`都会继承其中的样式。

现在你已经更好地掌握了何时使用继承，以及继承有哪些突出的优点，接下来我们看看一些高级用法。

# 6-2. 继承的高级用法;

任何`css`规则都可以继承其他规则，几乎任何`css`规则也都可以被继承。大多数情况你可能只想对类使用继承，但是有些场合你可能想做得更多。最常用的一种高级用法是继承一个`html`元素的样式。尽管默认的浏览器样式不会被继承，因为它们不属于样式表中的样式，但是你对`html`元素添加的所有样式都会被继承。

接下来的这段代码定义了一个名为`disabled`的类，样式修饰使它看上去像一个灰掉的超链接。通过继承a这一超链接元素来实现：

```
.disabled {
  color: gray;
  @extend a;
}
```

假如一条样式规则继承了一个复杂的选择器，那么它只会继承这个复杂选择器命中的元素所应用的样式。举例来说， 如果`.seriousError``@extend``.important.error` ， 那么`.important.error` 和`h1.important.error` 的样式都会被`.seriousError`继承， 但是`.important`或者`.error下`的样式则不会被继承。这种情况下你很可能希望`.seriousError`能够分别继承`.important`或者`.error`下的样式。

如果一个选择器序列（`#main .seriousError`）`@extend`另一个选择器（`.error`），那么只有完全匹配`#main .seriousError`这个选择器的元素才会继承`.error`的样式，就像单个类 名继承那样。拥有`class="seriousError"`的`#main`元素之外的元素不会受到影响。

像`#main .error`这种选择器序列是不能被继承的。这是因为从`#main .error`中继承的样式一般情况下会跟直接从`.error`中继承的样式基本一致，细微的区别往往使人迷惑。



现在你已经了解了通过继承能够做些什么事情，接下来我们将学习继承的工作细节，在生成对应`css`的时候，`sass`具体干了些什么事情。

# 6-3. 继承的工作细节;

跟变量和混合器不同，继承不是仅仅用`css`样式替换@extend处的代码那么简单。为了不让你对生成的`css`感觉奇怪，对这背后的工作原理有一定了解是非常重要的。

`@extend`背后最基本的想法是，如果`.seriousError @extend .error`， 那么样式表中的任何一处`.error`都用`.error``.seriousError`这一选择器组进行替换。这就意味着相关样式会如预期那样应用到`.error`和`.seriousError`。当`.error`出现在复杂的选择器中，比如说`h1.error``.error a`或者`#main .sidebar input.error[type="text"]`，那情况就变得复杂多了，但是不用担心，`sass`已经为你考虑到了这些。

关于`@extend`有两个要点你应该知道。

- 跟混合器相比，继承生成的`css`代码相对更少。因为继承仅仅是重复选择器，而不会重复属性，所以使用继承往往比混合器生成的`css`体积更小。如果你非常关心你站点的速度，请牢记这一点。
- 继承遵从`css`层叠的规则。当两个不同的`css`规则应用到同一个`html`元素上时，并且这两个不同的`css`规则对同一属性的修饰存在不同的值，`css`层叠规则会决定应用哪个样式。相当直观：通常权重更高的选择器胜出，如果权重相同，定义在后边的规则胜出。

混合器本身不会引起`css`层叠的问题，因为混合器把样式直接放到了`css`规则中，而继承存在样式层叠的问题。被继承的样式会保持原有定义位置和选择器权重不变。通常来说这并不会引起什么问题，但是知道这点总没有坏处。

# 6-4. 使用继承的最佳实践;

通常使用继承会让你的`css`美观、整洁。因为继承只会在生成`css`时复制选择器，而不会复制大段的`css`属性。但是如果你不小心，可能会让生成的`css`中包含大量的选择器复制。

避免这种情况出现的最好方法就是不要在`css`规则中使用后代选择器（比如`.foo .bar`）去继承`css`规则。如果你这么做，同时被继承的`css`规则有通过后代选择器修饰的样式，生成`css`中的选择器的数量很快就会失控：

```
.foo .bar { @extend .baz; }
.bip .baz { a: b; }
```

在上边的例子中，`sass`必须保证应用到.baz的样式同时也要应用到`.foo .bar`（位于class="foo"的元素内的class="bar"的元素）。例子中有一条应用到`.bip .baz`（位于class="bip"的元素内的class="baz"的元素）的`css`规则。当这条规则应用到`.foo .bar`时，可能存在三种情况，如下代码:

```
<!-- 继承可能迅速变复杂 -->
<!-- Case 1 -->
<div class="foo">
  <div class="bip">
    <div class="bar">...</div>
  </div>
</div>
<!-- Case 2 -->
<div class="bip">
  <div class="foo">
    <div class="bar">...</div>
  </div>
</div>
<!-- Case 3 -->
<div class="foo bip">
  <div class="bar">...</div>
</div>
```

为了应付这些情况，`sass`必须生成三种选择器组合（仅仅是.bip .foo .bar不能覆盖所有情况）。如果任何一条规则里边的后代选择器再长一点，`sass`需要考虑的情况就会更多。实际上`sass`并不总是会生成所有可能的选择器组合，即使是这样，选择器的个数依然可能会变得相当大，所以如果允许，尽可能避免这种用法。

值得一提的是，只要你想，你完全可以放心地继承有后代选择器修饰规则的选择器，不管后代选择器多长，但有一个前提就是，不要用后代选择器去继承。

# 7. 小结;

本文介绍了`sass`最基本部分,你可以轻松地使用`sass`编写清晰、无冗余、语义化的`css`。对于`sass`提供的工具你已经有了一个比较深入的了解，同时也掌握了何时使用这些工具的指导原则。

变量是`sass`提供的最基本的工具。通过变量可以让独立的`css`值变得可重用，无论是在一条单独的规则范围内还是在整个样式表中。变量、混合器的命名甚至`sass`的文件名，可以互换通用`_`和`-`。同样基础的是`sass`的嵌套机制。嵌套允许`css`规则内嵌套`css`规则，减少重复编写常用的选择器，同时让样式表的结构一眼望去更加清晰。`sass`同时提供了特殊的父选择器标识符`&`，通过它可以构造出更高效的嵌套。



你也已经学到了`sass`的另一个重要特性，样式导入。通过样式导入可以把分散在多个`sass`文件中的内容合并生成到一个`css`文件，避免了项目中有大量的`css`文件通过原生的`css` `@import`带来的性能问题。通过嵌套导入和默认变量值，导入可以构建更强有力的、可定制的样式。混合器允许用户编写语义化样式的同时避免视觉层面上样式的重复。你不仅学到了如何使用混合器减少重复，同时学习到了如何使用混合器让你的`css`变得更加可维护和语义化。最后，我们学习了与混合器相辅相成的选择器继承。继承允许你声明类之间语义化的关系，通过这些关系可以保持你的`css`的整洁和可维护性。