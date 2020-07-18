# bootStrap->组件->面包屑导航
## 目录
- 常用类名
	- <a href="#1-1">结构类名
	- <a href="#1-2">修改分隔符
	- <a href="#1-3">分隔符原理
- 代码示例
	-  <a href="#2-1">结构代码示例
## 常用类名
### <a id = "1-1">结构类名</a>
类名 | 描述
:--:|:--:
.breadcrumb|标记为面包屑导航
.breadcrumb-item|标记为面包屑导航内容项
.active|表示当前页面（高亮）
#### 无障碍处理  
属性 | 描述  
:--:|:--:  
aria-label="breadcrumb"| 针对面包屑这样具备导航功能的模块，添加一个有意义的标签来描述<nav>元素
aria-current="page"|这组导航的最后一个项目，以标明当前页面名称（路径）
### <a id = "1-2">修改分隔符</a>
//第一种
~~~
$breadcrumb-divider: quote(">");
~~~
//第二种，也可以使用base64嵌入式SVG图标：
~~~
$breadcrumb-divider: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI4IiBoZWlnaHQ9IjgiPjxwYXRoIGQ9Ik0yLjUgMEwxIDEuNSAzLjUgNCAxIDYuNSAyLjUgOGw0LTQtNC00eiIgZmlsbD0iY3VycmVudENvbG9yIi8+PC9zdmc+);
~~~
分隔符可以通过设置$breadcrumb-divider为none：
~~~
$breadcrumb-divider: none;
~~~
### <a id = "1-3">分隔符原理</a>
~~~
.breadcrumb-item + .breadcrumb-item::before {
  display: inline-block;
  padding-right: 0.5rem;
  color: #6c757d;
  content: "/";
}
~~~
## 代码示例
### <a id = "2-1">结构代码示例</a>
~~~
//标准格式
<nav aria-label="breadcrumb">
  <ol class="breadcrumb">
    <li class="breadcrumb-item active" aria-current="page">Home</li>
  </ol>
</nav>

<nav aria-label="breadcrumb">
  <ol class="breadcrumb">
    <li class="breadcrumb-item"><a href="#">Home</a></li>
    <li class="breadcrumb-item active" aria-current="page">Library</li>
  </ol>
</nav>

<nav aria-label="breadcrumb">
  <ol class="breadcrumb">
    <li class="breadcrumb-item"><a href="#">Home</a></li>
    <li class="breadcrumb-item"><a href="#">Library</a></li>
    <li class="breadcrumb-item active" aria-current="page">Data</li>
  </ol>
</nav>
~~~
~~~
//简单结构示例1(使用列表形式)
<ol class="breadcrumb">
  <li class="breadcrumb-item active">Home</li>
</ol>
<ol class="breadcrumb">
  <li class="breadcrumb-item"><a href="#">Home</a></li>
  <li class="breadcrumb-item active">Library</li>
</ol>
<ol class="breadcrumb">
  <li class="breadcrumb-item"><a href="#">Home</a></li>
  <li class="breadcrumb-item"><a href="#">Library</a></li>
  <li class="breadcrumb-item active">Data</li>
</ol>
~~~
~~~
//简单结构示例2(不使用列表形式)
<nav class="breadcrumb">
  <a class="breadcrumb-item" href="#">Home</a>
  <a class="breadcrumb-item" href="#">Library</a>
  <a class="breadcrumb-item" href="#">Data</a>
  <span class="breadcrumb-item active">Bootstrap</span>
</nav>
~~~