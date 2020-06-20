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