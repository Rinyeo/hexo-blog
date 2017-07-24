---
title: css/sass 规范
date: 2017/07/16 22:26:30
tags:
    - css
---


<!-- more -->

为什么我们需要一个css/sass 规范？

- 在代码库中设置代码质量标准;
- 提高代码库的一致性;
- 给开发团队一个熟悉的代码库感觉;
- 提高生产率。

## 语法和格式(Syntax and Formatting)


### 使用4个空白符来代替tab

如果你的编辑器支持tab等于4个空白符，可以使用tab。
不好的实践：

```scss
.nav {
  nav-tab {
    ...
  }
}

.nav {
      nav-tab {
        ...
      }
}
```


最佳实践：

```css
.nav {
    nav-tab {
        ...
   }
}

.nav {
    nav-tab {
	    ...
    }
}
```



### 使用多行CSS

不好的实践：

```css
header { color: red; height: 10px}
```

最佳实践：

```css
header {
    color: red;
    height: 10px;
}
```


### 使用有意义的标题

最佳实践：

```css
/*------------------------------------*\
  #SECTION-醒目标题
\*------------------------------------*/

.selector { }
```
样式表中一旦超过100多行，阅读和查找就会造成一定的困难，通过加上的注释和标记，可以提高阅读和查找效率。



### 使用有意义的空白符

在 sass 使用 1空白行/5空白行 来增强语义，相当于中文中的句子、段落，具体效果如下。

不好的实践，大部分人会按照这样方式来写。

```css
header {
    .nav {

    }
    .menu {

    }
}
footer {
    .copyright {

    }
    .date {
        
    }
}
```

最佳实践：

```css
/*------------------------------------*\
  #SECTION-Header
\*------------------------------------*/
header {
    .nav {

    }

    .menu {

    }
}




/*------------------------------------*\
  #SECTION-Footer
\*------------------------------------*/
footer {
    .copyright {

    }

    .date {
        
    }
}
```
.nav 与 .menu 之间空一行，.copyright 与.date 之间空一行，代表着句与句之间标识。header 和 footer 之间空五行，代表着段落与段落之间的标识。这样的样式书写看起来清爽，可阅读性和可维护性强。




## 命名规则(Naming Conventions)

### 1. 定义公共样式
在定义全局样式或公共样式时，请以 `g-`开头（代表着global），例如 `g-header` 、`g-nav`，通过这个标识符我们可以判断哪些样式是全局自定义样式，哪些是普通样式，例如自定义一个 swiper 组件样式。

```css
/*------------------------------------*\
  #SECTION-Swiper
\*------------------------------------*/
.g-swiper {
    .g-swiper-container {
        ...
    }

    .g-swiper-item {
        ...
    }
}
```

如果恰好该项目中引入了一个 swiper 插件，有一个 `home.html` 页面需要使用 swiper 插件，同时又需要使用自定义 swiper 组件，也不必担心样式冲突，同时后来维护的开发人员可以一眼认出`g-`开头的样式是全局自定义的：

```html
<div class="swiper">
    <ul class="swiper-container">
        <li class="swiper-item"></li>
        <li class="swiper-item"></li>
    <ul>
</div>





<!-- 通过标识符我们一眼可以断定这个样式是自定义样式 -->
<div class="g-swiper">
    <ul class="g-swiper-container">
        <li class="g-swiper-item"></li>
        <li class="g-swiper-item"></li>
    <ul>
</div>
```

### 2.重写公共样式
很多情况下公共样式并不能满足我们的需要，比如字体颜色，大小，或者偏移位置，但是整体的结构又差不多，只需要覆盖某个样式时，请不要让页面的样式依赖于全局类名，先看下面不好的实践：

```html
<!-- home.html -->

<!-- 重写 wsiper 默认背景颜色 -->
<style>

    .home .g-swiper {
        background: red;
    }
	
</style>





<body class="home">
	<div class="g-swiper"></div>
</body>

```

为什么这是一个不好的实践：

- 第一个方面是解耦，重写不应该依赖于 `g-swiper` 这个类名；如果需求上这里要改成垂直滚动，我们除了新增一个垂直滚动的组件类 `g-swiper-vertical`，并替换掉之前的类名之外，还需要去修改重写样式的类名（.home .g-swiper）。

- 第二个方面是阅读和调试性，通过查看浏览器控制台或者查看 html 文件不能够直观的看出重写。

解决方案：在需要重写样式的时候新增一个类名，并加上前缀 `ex-`，代表着 `extend` 在 Java 中是继承的意思，继承后可以重写父类样式。大多数人都头疼 CSS 命名，所以这里新起类名的时候直接 使用 `ex-` + `原类名`，例如 `g-swiper` 重写命名应该为 `ex-swiper`，而不能叫 `ex-g-swiper`。

```html
<!-- home.html -->

<!-- 重写 wsiper 默认背景颜色，这里依赖于ex-swiper -->
<style>

    .home .ex-swiper {
        background: red;
    }
    
</style>





<body class="home">
    <div class="g-swiper ex-swiper"></div>
</body>
```


### 3. 扩展公共样式
公共样式只是抽象出一部分相似的样式，所以在完善UI细节的时候，规则上 **扩展** 和 **重写** 一样，也使用 `ex-` 前缀，同时不需要为取新名字费神。


### 4. 防止命名冲突

如果是普通 jQuery 项目，请在 <body> 标签上添加上以 `vi-` 前缀开头的独立类名，例如 `vi-home`，vi 代表着 view;

如果是 ionic 项目，请在最外层 `<ion-view>` 标签上添加上以 `vi-` 前缀开头的独立类名。

 ```
    <ion-view view-title="首页" class="vi-home">
 ```

`vi-home` 的作用是用来划分 css 样式作用域

```scss
// home 模块
.vi-home {

    /*------------------------------------*\
    #SECTION-module1
    \*------------------------------------*/
    .module1 {
        .container {

        }
    }





    /*------------------------------------*\
    #SECTION-module2
    \*------------------------------------*/
    .module2 {
        .container {

        }
    }
}
```

### 5. 私有样式 CSS 命名方法
CSS 命名规则推荐BEM命名规则的设计思想，即 block + element + modifier。

- block 代表这个 UI 块是什么，header，nav，infoCard，menu，button 等。

- element 限定 block 是什么，container，content，item，list，row 等。

- modifier 修饰 block/element 的状态，error，left，hover，primary，mini 等。

在命名中 block，block + element ， block + modifier， block + element + modifier 多重组合使用。`Bootstrap` 中的命名也是参考BEM的这样的规则，我们可以以 `Bootstrap` 为例子。

**block:** `.nav` `.table` `.btn`

**block + modifier:** `.nav-stacked` `.tab-striped ` `.btn-primary`

**block + element:** `.nav-tabs` `.dropdown-menu` `.btn-group` `.btn-toolbar`

**block + modifier + modifier:** `.navbar-static-top`

**block + element + modifier:** `.input-group-addon` `.form-control-static`

> 同理在自定义全局样式也应该遵守这样的规则，同时在前面加上 `g-`。 `Bootstrap` 的源码非常的优秀，十分推荐大家去阅读。







## SASS 继承
当两个页面中页面是共用的情况时，推荐使继承的方式，把需要继承的样式放在 extend.scss 中，其他页面去调用，同时请在继承页面写清楚注释，例如：

```scss
// extend.scss

/* home & product view common banner，
 * 标识 home 和 product 两个视图共用一个 banner
*/

%banner {
    ...
}

```

```scss
// home.scss

.home {

    .banner {
        @extend %banner;
    }
}

```

```scss
// product.scss

.product {

    .banner {
        @extend %banner;
    }
	
}

```

如果要求 home 的 banner 要单独更改，而 product 中的不需要改变，那么我们只需要覆盖 home 继承中的样式:

```scss
// home.scss

.home {

    .banner {
        @extend %banner;	

        // overwrite 在下方重写继承的样式
        ...
        ...
    }
}

```

如果要求所有的 banner 图都做修改，直接修改 `extend.scss`即可，所以在 UI 改版的时候，需要过一遍需求，慎重修改。

## JavaScript 钩子
如果 JavaScript 中涉及到了 class id，那么请为这些 name 添加一个`js-`前缀来申明这个 class id 是供给 JS 来使用的，这样可以区分，哪些是样式，哪些是逻辑。

```html
<div class="js-date-content date-content"></div>

$('.js-date-content').text('.js-date-conten 这个类名是给 JS 来使用的');
```

## 增强 HTML 可阅读性
通过使用有意义的空白符来贼强 HTML 的可读性。在html中 1个空行代表逗号，2个空行代表句号，5个空行代表段落。5个空行推荐在最外层嵌套的大块之间使用，如果嵌套太深推荐在第二层嵌套中使用2个空行，否则使用1个空行。第3层嵌套之后都使用1个空行来分隔。同时和 sass 一样，为最外层的模块添加注释。
注意：最深的层级之间不需要空行，如果子节点只有一个标签，即无兄弟标签，也不需要空行，看如下示例


```html
<!-- header module  -->
<header class="page-head">
    <nav>
    	...
    </nav>

    <div class="page-logo">
    	...
    </div>
        
    <div class="page-menu">
	    ...
    </div>

</header>





<!-- body module -->
<main class="page-content">
    <!-- swiper  -->
    <div class="g-swiper">
        <ul class="g-swiper-container">
            <li class="g-swiper-item"></li>
            <li class="g-swiper-item"></li>
        <ul>
    </div>


    <!-- list-card  -->
    <div class="list-card">
        <ul class="list-card-container">
            <li class="list-card-item"></li>
            <li class="list-card-item"></li>
        <ul>
    </div>

</main>





<!-- footer module  -->
<footer class="page-foot">
  ...
</footer>
```

具体怎样使用空白符，请看个人视觉效果，调整到代码看起来清晰整洁即可。

H5新增的标签其实也是增强语义化，增加可读性，通过空行的方式让 html 的书写仿佛像写文章一样，层级结构分明，也是遵循 H5 的规范之一。