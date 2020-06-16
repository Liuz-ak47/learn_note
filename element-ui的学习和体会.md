### element-ui的学习和体会

#### Container 布局容器

用于布局的容器组件，方便快速搭建页面的基本结构：

`<el-container>：外层容器。当子元素中包含 `<el-header> 或 <el-footer>时，全部子元素会垂直上下排列，否则会水平左右排列。

<el-header>：顶栏容器。

<el-aside>：侧边栏容器。

<el-main>：主要区域容器。

<el-footer>：底栏容器。

以上组件采用了 flex 布局，使用前请确定目标浏览器是否兼容。此外，<el-container>的子元素只能是后四者，后四者的父元素也只能是 <el-container>。

*心得： 因为布局容器采用的是flex布局，所以当兄弟元素有一个比如<el-aside>设置了固定的宽度值width: 200px;，那么他的兄弟元素<el-main>或者<el-container>就只能占有剩下的全部空间。 <el-header>和<el-footer>一定占满容器元素<el-container>*

####  Layout 布局

类似于bootstrap的栅格系统。将页面分为24栏，进行布局。且支持flex布局和响应式。

可以在Layout布局中嵌套container布局容器。

基本用法：

<el-row>是一行，其中的<el-col>是列，父<el-row>通过gutter属性设置列之间的距离px，子<el-col>通过span属性设置列宽占24份的多少

```
<el-row :gutter="20">
	<el-col :span="8"><div class="grid-content bg-purple-dark"><div></el-col>
	<el-col :span="16"><div class="grid-content bg-purple-dark"><div></el-col>
<el-row>
```

还可以指定列的偏移量：

通过子<el-col>offset设置列的偏移量

```
<el-row :gutter="20">
  <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
  <el-col :span="6" :offset="6"><div class="grid-content bg-purple"></div></el-col>
</el-row>
```

还可以灵活的设置列的排序方式：

指定子项为flex布局，只需要父添加type="flex"，justify="start/ center/ end/ space-between/ space-around"

```
<el-row type="flex" justify="space-between">
	<el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
	<el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
</el-row>
```

还可以对列进行响应式布局，参照bootstrap的响应式设计，根据媒体查询将屏幕分成xs-768-sm-992-md-1200-lg-1920-xl五个档位，通过绑定这些尺寸的份数，设置列的大小。

````
<el-row :gutter="10">
  <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple"></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple-light"></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple"></div></el-col>
  <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple-light"></div></el-col>
</el-row>
````

还可以设置某些特定尺寸下，某列隐藏。需要注意如果不是完整引入的element-ui组件，使用隐藏功能时，需要单独引入样式文件，因为隐藏功能是通过给列添加类实现的。

````
import 'element-ui/lib/theme-chalk/display.css';
hidden-sm-only - 当视口在 sm 尺寸时隐藏
hidden-sm-and-down - 当视口在 sm 及以下尺寸时隐藏
hidden-sm-and-up - 当视口在 sm 及以上尺寸时隐藏
````









