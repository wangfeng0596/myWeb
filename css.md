# CSS 代码风格

## 基本设置

* 【强制】4 空格缩进,不允许2空格或者tab字符
* 【强制】UTF-8 编码

## 空白与格式

* 【强制】大括号与选择器之间留空，冒号后面留空，注释内外前后留空。<br/>
   理由：据说这样比较漂亮。
```CSS
/* 我是注释 */
div { /* 我是注释 */ }
span {
  color: red; /* 我是注释 */
}
```
* 【建议】在只有一条样式时允许和选择器写到同一行，不强制。<br/>
   理由：写三行太占屏幕空间。
* 【强制】一个选择器中有多个样式声明时每条写一行。<br/>
   理由：使报错可以精确到具体的规则上，便于排错。
* 【强制】多个选择器使用逗号隔开时写在不同的行，大括号不要另起一行。<br/>
   理由：修改时不容易漏掉逗号后面的选择器。<br>
* 【强制】声明块的右花括号应当单独成行
```CSS
div,
span {
  color: red;
  font-size: 12px;
}
```
* 【强制】每条样式声明后面都加上那个分号。<br/>
   理由：复制起来方便。
* 【建议】所有最外层引号使用双引号。<br/>
   理由：与 HTML 保持一致。
* 【建议】用逗号分隔的多个样式值写成多行。<br/>
   理由：便于阅读与编辑。
```CSS
.artical {
  font-family: "Vivien Leigh", victoria, female;
}
.block {
  box-shadow: 0 0 0 rgba(#000, 0.1),
              1px 1px 0 rgba(#000, 0.2),
              2px 2px 0 rgba(#000, 0.3),
              3px 3px 0 rgba(#000, 0.4),
              4px 4px 0 rgba(#000, 0.5);
}
```
* 【建议】每行不得超过 120 个字符，除非单行不可分割，常见不可分割的场景为URL超长。<br/>
   对于超长的样式，在样式值的 空格 处或 , 后换行，建议按逻辑分组
```CSS
/* 不同属性值按逻辑分组 */
background:
    transparent url(aVeryVeryVeryLongUrlIsPlacedHere)
    no-repeat 0 0;

/* 可重复多次的属性，每次重复一行 */
background-image:
    url(aVeryVeryVeryLongUrlIsPlacedHere)
    url(anotherVeryVeryVeryLongUrlIsPlacedHere);

/* 类似函数的属性值可以根据函数调用的缩进进行 */
background-image: -webkit-gradient(
    linear,
    left bottom,
    left top,
    color-stop(0.04, rgb(88,94,124)),
    color-stop(0.52, rgb(115,123,162))
);
```
* 【强制】属性选择器中的值必须用双引号包围,不允许使用单引号
```CSS
/* good */
article[character="juliet"] {
    voice-family: "Vivien Leigh", victoria, female
}

/* bad */
article[character='juliet'] {
    voice-family: "Vivien Leigh", victoria, female
}
```
## 通用
### 选择器
* 【建议】选择器的嵌套层级应不大于 3 级，位置靠后的限定条件应尽可能精确。
```CSS
/* godd */
.container .article {}

/* bad */
.container .article .user .name {}
```
### 属性缩写
* 【建议】在可以使用缩写的情况下，尽量使用属性缩写。
```CSS
/* good */
.post {
    font: 12px/1.5 arial, sans-serif;
}

/* bad */
.post {
    font-family: arial, sans-serif;
    font-size: 12px;
    line-height: 1.5;
}
```
* 【建议】使用 border / margin / padding 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写。<br>
   理由： border / margin / padding 等缩写会同时设置多个属性的值，容易覆盖不需要覆盖的设定。如某些方向需要继承其他声明的值，则应该分开设置。
```CSS
/* centering <article class="page"> horizontally and highlight featured ones */
article {
    margin: 5px;
    border: 1px solid #999;
}

/* good */
.page {
    margin-right: auto;
    margin-left: auto;
}

.featured {
    border-color: #69c;
}

/* bad */
.page {
    margin: 5px auto; /* introducing redundancy */
}

.featured {
    border: 1px solid #69c; /* introducing redundancy */
}
```
### 属性书写顺序
* 【建议】同一 rule set 下的属性在书写时，应按功能进行分组，并以 Formatting Model（布局方式、位置） > Box Model（尺寸） > Typographic（文本相关） > Visual（视觉效果） 的顺序书写，以提高代码的可读性。<br>
   如果包含 content 属性，应放在最前面<br>
   Formatting Model 相关属性包括：position / top / right / bottom / left / float / display / overflow 等<br>
   Box Model 相关属性包括：border / margin / padding / width / height 等<br>
   Typographic 相关属性包括：font / line-height / text-align / word-wrap 等<br>
   Visual 相关属性包括：background / color / transition / list-style 等<br>
```CSS
.sidebar {
    content:"",
    /* formatting model: positioning schemes / offsets / z-indexes / display / ...  */
    position: absolute;
    top: 50px;
    left: 0;
    overflow-x: hidden;

    /* box model: sizes / margins / paddings / borders / ...  */
    width: 200px;
    padding: 5px;
    border: 1px solid #ddd;

    /* typographic: font / aligns / text styles / ... */
    font-size: 14px;
    line-height: 20px;

    /* visual: colors / shadows / gradients / ... */
    background: #f5f5f5;
    color: #333;
    -webkit-transition: color 1s;
       -moz-transition: color 1s;
            transition: color 1s;
}
```
### 清除浮动
* 【建议】 当元素需要撑起高度以包含内部的浮动元素时，通过对伪类设置 clear 或触发 BFC 的方式进行 clearfix。尽量不使用增加空标签的方式。<br>
   触发 BFC 的方式很多，常见的有：<br>
   float 非 none<br>
   position 非 static<br>
   overflow 非 visible<br>
### !important 
* 【建议】 尽量不使用 !important 声明。
* 【建议】 当需要强制指定样式且不允许任何场景覆盖时，通过标签内联和 !important 定义样式。<br>
   解释：必须注意的是，仅在设计上 确实不允许任何其它场景覆盖样式 时，才使用内联的 !important 样式。通常在第三方环境的应用中使用这种方案。下面的 z-index 章节是其中一个特殊场景的典型样例。<br>
### z-index
* 【建议】将 z-index 进行分层，对文档流外绝对定位元素的视觉层级关系进行管理。<br>
  解释：同层的多个元素，如多个由用户输入触发的 Dialog，在该层级内使用相同的 z-index 或递增 z-index。建议每层包含100个 z-index 来容纳足够的元素，如果每层元素较多，可以调整这个数值。<br>

* 【建议】在可控环境下，期望显示在最上层的元素，z-index 指定为 999999。<br>
   解释：可控环境分成两种，一种是自身产品线环境；还有一种是可能会被其他产品线引用，但是不会被外部第三方的产品引用。不建议取值为 2147483647。以便于自身产品线被其他产品线引用时，当遇到层级覆盖冲突的情况，留出向上调整的空间。

* 【建议】在第三方环境下，期望显示在最上层的元素，通过标签内联和 !important，将 z-index 指定为 2147483647。<br>
   解释：第三方环境对于开发者来说完全不可控。在第三方环境下的元素，为了保证元素不被其页面其他样式定义覆盖，需要采用此做法。<br>

## 值与单位
### 文本
* 【强制】文本内容必须用双引号包围。
```CSS
/* good */
html[lang|="zh"] q:before {
    font-family: "Microsoft YaHei", sans-serif;
    content: "“";
}

html[lang|="zh"] q:after {
    font-family: "Microsoft YaHei", sans-serif;
    content: "”";
}

/* bad */
html[lang|=zh] q:before {
    font-family: 'Microsoft YaHei', sans-serif;
    content: '“';
}

html[lang|=zh] q:after {
    font-family: "Microsoft YaHei", sans-serif;
    content: "”";
}
```
### 数值
* 【强制】当数值为 0 - 1 之间的小数时，省略整数部分的 0。
```CSS
/* good */
panel {
    opacity: .8
}

/* bad */
panel {
    opacity: 0.8
}
```
### url
* 【强制】url() 函数中的路径不加引号。
* 【建议】url() 函数中的绝对路径可省去协议名。
```CSS
body {
    background: url(bg.png);
}
body {
    background: url(//baidu.com/img/bg.png) no-repeat 0 0;
}
```
### 长度
* 【建议】 0 值的单位建议省略，但不强制<br>
```CSS
/* good */
body {
    padding: 0 5px;
}

/* bad */
body {
    padding: 0px 5px;
}
```
### 颜色
* 【强制】RGB颜色值必须使用十六进制记号形式 #rrggbb。不允许使用 rgb()。<br>
   带有alpha的颜色信息可以使用 rgba()。使用 rgba() 时每个逗号后必须保留一个空格。<br>
```CSS
/* good */
.success {
    box-shadow: 0 0 2px rgba(0, 128, 0, .3);
    border-color: #008000;
}

/* bad */
.success {
    box-shadow: 0 0 2px rgba(0,128,0,.3);
    border-color: rgb(0, 128, 0);
}
```
* 【建议】颜色值可以缩写时，建议使用缩写形式。
```CSS
/* good */
.success {
    background-color: #aca;
}

/* bad */
.success {
    background-color: #aaccaa;
}
```
* 【强制】 颜色值不允许使用命名色值。
* 【建议】 颜色值中的英文字符采用小写。
```CSS
/* good */
.success {
    color: #90ee90;
    background-color: #aca;
}

/* bad */
.success {
    color: lightgreen;
    background-color: #ACA;
}
```
### 2D 位置
* 【强制】必须同时给出水平和垂直方向的位置。
   解释：2D 位置初始值为 0% 0%，但在只有一个方向的值时，另一个方向的值会被解析为 center。为避免理解上的困扰，应同时给出两个方向的值。background-position属性值的定义。<br>
```CSS
/* good */
body {
    background-position: center top; /* 50% 0% */
}

/* bad */
body {
    background-position: top; /* 50% 0% */
}
```

## 文本编排
### 字体族
* 【强制】 font-family 属性中的字体族名称应使用字体的英文 Family Name，其中如有空格，须放置在引号中。<br>
```CSS
h1 {
    font-family: "Microsoft YaHei";
}
```
* 【强制】 font-family 按「西文字体在前、中文字体在后」、「效果佳 (质量高/更能满足需求) 的字体在前、效果一般的字体在后」的顺序编写，最后必须指定一个通用字体族( serif / sans-serif )。<br>
```CSS
/* Display according to platform */
.article {
    font-family: Arial, sans-serif;
}

/* Specific for most platforms */
h1 {
    font-family: "Helvetica Neue", Arial, "Hiragino Sans GB", "WenQuanYi Micro Hei", "Microsoft YaHei", sans-serif;
}
```
* 【强制】 font-family 不区分大小写，但在同一个项目中，同样的 Family Name 大小写必须统一。<br>
```CSS
/* good */
body {
    font-family: Arial, sans-serif;
}

h1 {
    font-family: Arial, "Microsoft YaHei", sans-serif;
}

/* bad */
body {
    font-family: arial, sans-serif;
}

h1 {
    font-family: Arial, "Microsoft YaHei", sans-serif;
}
```
### 字号
* 【强制】需要在 Windows 平台显示的中文内容，其字号应不小于 12px。<br>
   解释：由于 Windows 的字体渲染机制，小于 12px 的文字显示效果极差、难以辨认。
### 字体风格 
* 【建议】 需要在 Windows 平台显示的中文内容，不要使用除 normal 外的 font-style。其他平台也应慎用。<br>
   解释：由于中文字体没有 italic 风格的实现，所有浏览器下都会 fallback 到 obilique 实现 (自动拟合为斜体)，小字号下 (特别是 Windows 下会在小字号下使用点阵字体的情况下) 显示效果差，造成阅读困难。<br>
### 字重
* 【强制】font-weight 属性必须使用数值方式描述。<br>
   解释：<br>
   CSS 的字重分 100 – 900 共九档，但目前受字体本身质量和浏览器的限制，实际上支持 400 和 700 两档，分别等价于关键词 normal 和 bold。<br>

   浏览器本身使用一系列启发式规则来进行匹配，在 =700 时匹配 Bold 字重。<br>

   但已有浏览器开始支持 =600 时匹配 Semibold 字重 (见此表)，故使用数值描述增加了灵活性，也更简短。<br>
```CSS
/* good */
h1 {
    font-weight: 700;
}

/* bad */
h1 {
    font-weight: bold;
}
```
### 行高
* 【建议】line-height 在定义文本段落时，应使用数值。<br>
   解释：<br>
   将 line-height 设置为数值，浏览器会基于当前元素设置的 font-size 进行再次计算。在不同字号的文本段落组合中，能达到较为舒适的行间间隔效果，避免在每个设置了 font-size 都需要设置 line-height。<br>

   当 line-height 用于控制垂直居中时，还是应该设置成与容器高度一致。<br>
```CSS
.container {
    line-height: 1.5;
}
```
## 变换与动画
* 【强制】使用 transition 时应指定 transition-property。<br>

```CSS
/* good */
.box {
    transition: color 1s, border-color 1s;
}

/* bad */
.box {
    transition: all 1s;
}
```
* 【建议】尽可能在浏览器能高效实现的属性上添加过渡和动画。<br>
   在可能的情况下应选择这样四种变换：<br>
   transform: translate(npx, npx);<br>
   transform: scale(n);<br>
   transform: rotate(ndeg);<br>
   opacity: 0..1;<br>
   典型的，可以使用 translate 来代替 left 作为动画属性。<br>
```CSS
/* good */
.box {
    transition: transform 1s;
}
.box:hover {
    transform: translate(20px); /* move right for 20px */
}

/* bad */
.box {
    left: 0;
    transition: left 1s;
}
.box:hover {
    left: 20px; /* move right for 20px */
}
```
## 响应式
* 【强制】 Media Query 不得单独编排，必须与相关的规则一起定义。<br>

```CSS
/* Good */
/* header styles */
@media (...) {
    /* header styles */
}

/* main styles */
@media (...) {
    /* main styles */
}

/* footer styles */
@media (...) {
    /* footer styles */
}


/* Bad */
/* header styles */
/* main styles */
/* footer styles */

@media (...) {
    /* header styles */
    /* main styles */
    /* footer styles */
}
```

* 【强制】Media Query 如果有多个逗号分隔的条件时，应将每个条件放在单独一行中。<br>

```CSS
@media
(-webkit-min-device-pixel-ratio: 2), /* Webkit-based browsers */
(min--moz-device-pixel-ratio: 2),    /* Older Firefox browsers (prior to Firefox 16) */
(min-resolution: 2dppx),             /* The standard way */
(min-resolution: 192dpi) {           /* dppx fallback */
    /* Retina-specific stuff here */
}
```

* 【建议】尽可能给出在高分辨率设备 (Retina) 下效果更佳的样式。

## 兼容性
* 【强制】带私有前缀的属性由长到短排列，按冒号位置对齐。<br>
   解释：标准属性放在最后，按冒号对齐方便阅读，也便于在编辑器内进行多行编辑。<br>
   
```CSS
.box {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
}
```

* 【建议】尽量使用 选择器 hack 处理兼容性，而非 属性 hack。<br>
  解释：尽量使用符合 CSS 语法的 selector hack，可以避免一些第三方库无法识别 hack 语法的问题。<br>
  
``` CSS
/* IE 7 */
*:first-child + html #header {
    margin-top: 3px;
    padding: 5px;
}

/* IE 6 */
* html #header {
    margin-top: 5px;
    padding: 4px;
}
```

* 【建议】尽量使用简单的 属性 hack。<br>

```CSS
.box {
    _display: inline; /* fix double margin */
    float: left;
    margin-left: 20px;
}

.container {
    overflow: hidden;
    *zoom: 1; /* triggering hasLayout */
}
```

* 【强制】禁止使用 Expression <br>
   理由：影响性能。
   
## 功能限定

* 避免使用 ID 选择器。<br/>
  理由：权重太高，不易维护。
* 禁止使用 @import 引入 CSS 文件。
  理由：各种坑不解释。<br/>

## 命名与模块化

* 类名中的字母一律小写。<br/>
  理由：它不区分大小写，难道有人想统一大写？
* 类名中只使用字母、数字以及“-”。<br/>
  理由：要是没有限制的话我怕一些猴子使用阿拉伯文。
```CSS
.hello {} /* OK */
.module-title {} /* OK */
.panel-level1 {} /* OK */
.导航栏 /* Fuck */
```
