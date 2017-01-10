# less 快速入门

## 总览

### 变量
less 中可以定义变量 (其实是常量)

```less
@nice-blue: #5b83ad;
@light-blue: @nice-blue + #111;

#header {
  color: @light-blue;
}
```

### Mixins
Mixins 将一系列的 css 属性规则混入到另外的规则中, 只需要添加需要的规则名.

```less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered;
}

.post a {
  color: red;
  .bordered;
}
```

### 嵌套规则
通过嵌套 css 规则, 可以更好的组织文件结构.

#### 指令嵌套
类似于 `media` 和 `keyframe` 之类的 css 指令, 在嵌套的时候会放到最外层.

### 操作符
算术操作符 `+, -, *, /` 可以用在任意的数字, 颜色或变量上, 可能的话就会自动进行单位转换.

```less
// numbers are converted into the same units
@conversion-1: 5cm + 10mm; // result is 6cm
@conversion-2: 2 - 3cm - 5mm; // result is -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // result is 4px

// example with variables
@base: 5%;
@filler: @base * 2; // result is 10%
@other: @base + @filler; // result is 15%
```

对于乘法和除法, 单位转换没有意义, 因此只会保留第一个数字的单位.

```less
@base = 3cm * 2mm; // result is 6cm 
```

对颜色的运算会分成 `r, g, b` 通道处理. 比如, 两个颜色相加, 那么结果的绿色通道会是两个颜色绿色的相加. (对 `alpha` 通道进行操作是未定义的)

### 转义
所有以 `~` 开头的字符串将不会经过 less 转换直接输出到 css 中.

### 函数

less 内置了处理颜色, 字符串和算术操作的函数.

### 命名空间
命令空间可以将嵌套的规则进行 Mixins.

``` less
#bundle {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white
    }
  }
  .tab { ... }
  .citation { ... }
}

// 使用命名空间 Mixins
#header a {
  color: orange;
  #bundle > .button;
}
```

命名空间中的变量是 `private` 的, 意味着不能引用命名空间中的变量, 即 `#Namespace > @this-will-not-work` 是不允许的.

### Scope
less 中的变量和规则查找类似编程语言, 即先从当前作用域开始查询, 然后查找父作用域.
less 中的变量和规则并不需要提前声明.

```
#page {
  @var: white;
  #header {
    color: @var; // 父作用域 white
  }
}

@var: red; // 不需要提前声明
```

### 注释
### @import


## 变量
变量不仅可以定义 css 中属性的值, 也可以用于选择器名, 属性名, url 和 @import 语句中

```less
// 用在选择器中
// Variables
@my-selector: banner;

// Usage
.@{my-selector} {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}

// 用在 url 中
// Variables
@images: "../img";

// Usage
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}

// 用在 @import 中
// Variables
@themes: "../../src/themes";

// Usage
@import "@{themes}/tidal-wave.less";

// 用在属性名中
// Variables
@property: color;

// Usage
.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}

// 还可以用在变量名中
@fnord:  "I am fnord.";
@var:    "fnord";
content: @@var;
```

编译成

```css
.banner {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}

body {
  color: #444;
  background: url("@{images}/white-sand.png");
}

@import "../../src/themes/tidal-wave.less"

.widget {
  color: #0ee;
  background-color: #999;
}

content: "I am fnord.";
```

### 惰性加载
less 中的变量是惰性加载的, 意味着只有在使用时才会去查找变量, 同时最后定义的变量生效

```less
.lazy-eval-scope {
  width: @var;
  @a: 9%;
}

@var: @a;
@a: 100%;
```

编译成

```css
.lazy-eval-scope {
  width: 9%;
}
```

## Extend
>> extend 是 less的伪类选择器，他会复制当前选择器，定义新的样式，而原来的不变. http://www.cnblogs.com/xiyangbaixue/p/4146657.html
extend 是另一种实现复用的方式.

## Mixins

在定义 mixin 时添加括号, 那么 mixin 就不会添加在生成的 css 中.
