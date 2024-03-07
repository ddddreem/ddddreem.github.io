

---
title:  前端学习笔记
date: 2023-3-7 15:53:11
tags: 

- other
- problems

---

# Html、CSS基础知识

## Html

### Html元素的理解

#### 单标签和双标签

```html
<br /> <!-- 最典型的单标签 -->

<p> 这是一个段落 </p> <!-- 最典型的双标签，需要有开始标签和结束标签 -->
```

#### 双标签的级别

根据标签内部可以存放的元素内容不同，可以将双标签划分为两个级别

- 容器级：标签内部可以存放任意内容，包含容器级标签。如h1，div等
- 文本级：标签内部只能存放文字或类似文字的内容，比如存放图片、表单元素等。如p等



### DTD（文档定义类型）

![image-20240120163300878](C:\Users\admin\Desktop\前端\imgs\image-20240120163300878.png)

![image-20240120163454600](C:\Users\admin\Desktop\前端\imgs\image-20240120163454600.png)

### 命名空间

![image-20240120163709886](C:\Users\admin\Desktop\前端\imgs\image-20240120163709886.png)

作用：统一标签的含义，避免相同标签在不同的浏览器中出现歧义；

### Html标签

#### 标题标签

容器级标签，且是双标签（有开头有结束）；

```html
<h1>一级标签</h1>
<h2>二级标签</h2>
<h3>三级标签</h3>
<h4>四级标签</h4>
<h5>五级标签</h5>
<h6>六级标签</h6>
```

#### 无序列表

<ul\>里面只能嵌套<li\>，

```html
<ul>
  <li>
    <h1>红楼梦</h1>
    <ul>
      <li>林黛玉</li>
      <li>薛宝钗</li>
      <li>王熙凤</li>
    </ul>
  </li>
  <li>三国演义</li>
  <li>西游记</li>
  <li>水浒传</li>
</ul>
```



![image-20240121105731346](C:\Users\admin\Desktop\前端\imgs\image-20240121105731346.png)



#### 有序列表

用法类似于无序列表

![image-20240121110413460](C:\Users\admin\Desktop\前端\imgs\image-20240121110413460.png)



#### 定义列表标签

![image-20240121110749299](C:\Users\admin\Desktop\前端\imgs\image-20240121110749299.png)

![image-20240121111357681](C:\Users\admin\Desktop\前端\imgs\image-20240121111357681.png)

#### 布局标签（div和span）

div偏向于大的布局，span更偏向于小范围的布局，比如是改变一个字的颜色等；

#### 表单标签

##### input标签

![image-20240121180032650](C:\Users\admin\Desktop\前端\imgs\image-20240121180032650.png)

##### label标签

![image-20240121210732141](C:\Users\admin\Desktop\前端\imgs\image-20240121210732141.png)

![image-20240121210914302](C:\Users\admin\Desktop\前端\imgs\image-20240121210914302.png)

如果绑定内容与表单元素离的比较远，可以考虑使用绑定方法一；

#### a标签

![image-20240126083658651](C:\Users\admin\Desktop\前端\imgs\image-20240126083658651.png)

## CSS

### 行内式样式表

```html
<p style="color='red';" > 你好 </p>
```



### 内嵌式样式表

内嵌式表示写在html文件的头部中的样式

```html
<head>
  <style type="text/css">
    p {
      color: red;
    }
  </style>
</head>
```





### 外联样式表引用

![image-20240122213630716](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20240122213630716.png)

![image-20240122214513727](C:\Users\admin\Desktop\前端\imgs\image-20240122214513727.png)

### 导入式样式表引用

![image-20240122214649084](C:\Users\admin\Desktop\前端\imgs\image-20240122214649084.png)

![image-20240122215057122](C:\Users\admin\Desktop\前端\imgs\image-20240122215057122.png)

### 样式书写规则

![image-20240122215341375](C:\Users\admin\Desktop\前端\imgs\image-20240122215341375.png)

![image-20240122220938366](C:\Users\admin\Desktop\前端\imgs\image-20240122220938366.png)

### CSS代码书写建议（书写规范）

![image-20240122224828298](C:\Users\admin\Desktop\前端\imgs\image-20240122224828298.png)

### 选择器

#### 基础选择器

##### 标签选择器



##### id选择器



##### 类选择器

![image-20240123223006929](C:\Users\admin\Desktop\前端\imgs\image-20240123223006929.png)

##### 通配符选择器

![image-20240123223541890](C:\Users\admin\Desktop\前端\imgs\image-20240123223541890.png)

![image-20240123223518912](C:\Users\admin\Desktop\前端\imgs\image-20240123223518912.png)

#### 高级选择器

##### 后代选择器

```html
<style type="text/css">
  .box1 ul li p {
    color: red;
  }
</style>

<div class="box1 fs12">
  <ul>
    <li><p>段落1</p></li>
    <li><p>段落2</p></li>
    <li><p>段落3</p></li>
    <li><p>段落4</p></li>
  </ul>
  <p>段落5</p>
</div>
```



![image-20240123224039542](C:\Users\admin\Desktop\前端\imgs\image-20240123224039542.png)

##### 交集选择器

```html
p.demo /* 表示选择的是p标签里class属性带有demo的 */
```



![image-20240124082754730](C:\Users\admin\Desktop\前端\imgs\image-20240124082754730.png)



##### 并集选择器



### css层叠式概念

#### css继承性



#### css层叠性                           

css样式存在层叠覆盖的特性，主要判断css的权重，权重高的样式会覆盖权重低的样式；

![image-20240125084611444](C:\Users\admin\Desktop\前端\imgs\image-20240125084611444.png)

![image-20240124225941476](C:\Users\admin\Desktop\前端\imgs\image-20240124225941476.png) 

![image-20240124231816054](C:\Users\admin\Desktop\前端\imgs\image-20240124231816054.png)  

![image-20240125083849723](C:\Users\admin\Desktop\前端\imgs\image-20240125083849723.png)



**important 属性可以提升某条属性的权重至最大**

只会影响某条属性的权重，不会改变选择器的权重；

```css
#box1 {
	color: red !important;
}
```

![image-20240125085036858](C:\Users\admin\Desktop\前端\imgs\image-20240125085036858.png)

![image-20240125085154443](C:\Users\admin\Desktop\前端\imgs\image-20240125085154443.png)
