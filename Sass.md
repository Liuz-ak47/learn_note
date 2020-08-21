### Sass

#### 安装

cnpm install sass -g 

#### 编译

sass的文件以.scss命名，需要被编译为.css后才能被html文件引用。

将input.css编译为output.css文件：

sass input.scss output.css

实时监视.scss文件，当保存时就能自动编译为.css文件：

sass --watch input.scss:output.css

实时监控文件夹中的.scss文件，保存时自动编译为.css：

sass --watch sourcedir:dist/outsource



### SCSS

#### scss文件的导入

```scss
 如果想导入一个scss文件，这个scss文件不被编译为css文件，这个文件需要命名为_开头，而导入它的那个文件不要加_。
 //_a.scss
 在b.scss中导入_a.scss文件，且要_.scss文件不被编译为css
 //b.scss
 @import 'a.scss'
```



#### 混入mixin :  定义一个公共的样式函数，多个选择器都可以使用

```scss
  //定义：
无变量：
@mixin alert{
	color: red;
}
或有变量(一个或多个)：
@mixin alert($color, $font-size){
	color: $color;
    font-size: $font-size;
}

  //使用
无变量
#app {
    @include alert;
}
有变量：
按照定义的顺序填写实参
#app {
    @include alert(red, 12px);
}
或手动指定每个实参，这样可以不按顺序
#app {
    @include alert($font-size: 13px, $color: green);
}
```

#### 继承

```scss
使用@extend关键字实现继承。简言之，他有的我也要有。
继承例子1:
.alert{
	color: red;
}
#app {
	@extend .alert;
}
将被编译为：
.alert{
	color: red;
}
#app {
	color: red;
}
继承例子2：
$color: pink;
.alert a {
    color: $color;
    text-decoration: none;
}
#app {
    @extend: .alert;
}
将被编译为：
.alert a{
    color: pink;
    text-decoration: none;
}
#app a {
    color: pink;
    text-decoration: none;    
}
```

#### 一些函数

```scss
//数字的：
abs(2px)取绝对值 2px
round(2.5)四舍五入 3
ceil(2.2)向上取整 3
floor(2.2)向下取整 2
percentage(650px / 1000px)取百分数 65%
min(1, 2, 3)取最小 1
max(1, 2, 3)取最大 3
//字符串的：
$hi: "NiHao" 
to-upper-case($hi) 转大写"NIHAO"
to-lower-case($hi) 转小写"nihao"
str-length($hi) 长度5
str-index($hi, "Hao") 开始位置3 注意和JS不同
str-insert($hi, "Ma", 6) 指定位置插入字符串"NiHaoMa"
//颜色的： 快捷键shift+ctrl+c
rgba 分别代表红 绿 蓝 0-255 透明度0-1
rgb(255, 0, 0) 红色
rgba(255, 0, 0, .8) 透明度0.8的红色
hsla 分别代表色相0-360 饱和度 明度0-100% 透明度0-1
hsl(0, 100%, 50%) 红色red
hsl(60, 100%, 50%)  黄色yellow
hsla(60, 100%, 50%, .5) 透明度为0.5的黄色
$base-color1: #ff0000; 定义两种颜色
$base-color-hsl: hsl(0, 100, 50%); 第二种颜色与第一种是同一种颜色
adjust-hue($base-color-hsl, 137deg); 将色相增加137度 #00ff48
adjust-hue($base-color1, 137deg); 得到上面同样的结果
$base-color2: hsl(221, 50%, 50%);
saturate($base-color2, 50%); 增加50%的饱和度 #0051ff
desaturage($base-color2, 30%); 减少30%的饱和度 #667699
$base-color3: hsl(222, 100%, 50%);
lighten($base-color3, 30%); 增加30%的明度 #99b8ff
darken($base-color3, 20%); 减小20%的明度 #002e99
$base-color4: hsla(222, 100%, 50%, .5);
opacify($base-color4, 0.3); 增加0.3不透明度 rgba(0, 76, 255, 0.8)
transparentize($base-color4,); 减少0.2透明度 rgba(0, 76, 255, 0.3)
//列表的：
length(5px solid blue); 列表长度是3
nth(5px solid blue, 2); 得到列表的第2项 solid
index(5px solid blue, solid); 得到solid在列表中的序号 2
append(5px solid, blue); 得到插入到最后的列表 (5px solid blue)
append(5px solid, blue); 指定分隔符 (5px, solid, blue)
join(5px 10px, 5px 0px); 将两个列表合并 (5px 10px 5px 0px)
join(5px 10px, 5px 0px, comma); 指定分隔符 (5px, 10px, 5px, 0px)
//map的：
$colors: (light: #ffffff, dark: #000000);
length($colors) 有两个数据 2
map-get($colors, light) 得到值#ffffff
map-keys($colors) 得到键的列表("light", "dark")
map-values($colors) 得到值的列表(#ffffff, #000000)
map-has-key($colors, light) 是否具有某个键 true
map-merge($colors, (light-gray: #1e22ff)) 将两个map合并 (light: #ffffff, dark: #000000, light-gray: #1e22ff)
map-remove($colors, light) 移除map中的特定的一个多多个键值对 (dark: #000000)
//布尔值：
(5px > 3px) and (5px != 10px) 得到true
(5px == 3px) or (5px < 10px) 得到true
not (5px > 3px) 得到false
//插值表达式 #{}
$version: "0.0.1";
/* 当前版本是：#{$version} */   编译为/* 当前版本是：0.0.1 */
$name: "info";
$attr: "border";
.alert-#{$name} {
    #{$attr}-color: #fff;
}
编译为：
.alert-info {
    border-color: #fff;
}
```

#### 一些指令

```scss
//条件指令@if 满足时执行，非false或null就可以
//例子
$use-prfixes: true; 
.rounded {
    @if $use-prfixes {
        border-radius:5px;
    }
}
$theme: "light";
body {
    @if $theme == dark {
        background-color: black;
    } @else if $theme == light {
        background-color: white;
    } @else {
        background-color: grey;
    }
}

//@for 循环值指令 两种形式
@for $var from <开始值> through <结束值> {} //循环包含结束值
@for $var from <开始值> to <结束值> {} //循环不包含结束值
//例子
$cols: 4;
@for $i from 1 through $cols {
    .col-#{$i}{
        width: 100% / $cols * $i;
    }
}
编译为：
.col-1 {
    width: 25%;
}
.col-2 {
    width: 50%;
}
.col-1 {
    width: 75%;
}
.col-1 {
    width: 100%;
}

//@each 循环列表、map指令 @each
@each $var in $list {}
//例子
定义一个列表
$icons: success error warning;
使用循环列表指令
@each $i in $icons {
    .icon-#{$i} {
        background-image: url(../images/icons/#{$i}.png);
    }
}
编译为：
.icon-success {
    background-image: url(../images/icons/success.png);
}
.icon-error {
    background-image: url(../images/icons/error.png);
}
.icon-warning {
    background-image: url(../images/icons/warning.png);
}

@each $header, $size in (h1: 2em, h2: 1.5em, h3: 1.2em) {
  #{$header} {
    font-size: $size;
  }
}
编译为：
h1 {
  font-size: 2em; }
h2 {
  font-size: 1.5em; }
h3 {
  font-size: 1.2em; }

//循环指令  类似@for
@while 条件 {}
//例子
$i: 2;
@while $i > 0 {
    .item-#{$i} {
        width: 5px * $i;
    }
    $i: $i - 1;  //改变$i 以停止循环 
}
编译为：
.item-2 {
    width: 10px;
}
.item-1 {
    width: 5px;
}
//函数指令 function
@function 函数名 (参数1, 参数2){}
//例子
$colors: (light: #fff, dark: #000);
@function color($key) {
    if not map-has-key($colors, $key) {
        @warn "在$colors中没有找到#{$key}"; //或者@error
    }
    @return map-get($colors, $key)
}
使用这个函数
body {
    background-color: color(light);
}    
编译为：
body {
    background-color: #fff;
}  

//那么循环map呢？
@function iteraorMap($map) {
    if type-of($map) != map {
        @error "#{$map}不是一个map";
    }
    $keys: map-keys($map);
    $strings: "";
    @for $i from 1 through length($keys) {
        $strings = $map +"的第"+ $i +"项的键是"+ nth($keys, $i) + ",值是"+ map-get($map,       nth($keys, $i)) + ", " + $strings
    } 
    @return $string;
} 
```


















































































