---
title: Markdown学习笔记
date: 2022-05-26 19:51:11
tags: 
- tech
- 工具语言
- MarkDown
---

# 学习Markdown语法

&lt;· #是标题前缀

### （1）插入本地图片链接

![这里写图片描述](https://img-blog.csdn.net/20180802155057285?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





### （2）插入互联网上的图片

**·引用图片用**

***eg:***

​	![asdf](http://pic.downcc.com/upload/2015-9/2015923174024.png)

![这里写图片描述](https://img-blog.csdn.net/20180802155336302?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

​	![markdown](http://pic.downcc.com/upload/2015-9/2015923174024.png)



+ Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用<>包起来， Markdown 就会自动把它转成链接。也可以直接写，也是可以显示成链接形式的

​	***eg:***

<https://baidu.com>

https://www.baidu.com/baidu.html?from=noscript



### 分隔符

·你可以在一行中用三个以上的<font color=red face="宋体" size=3>**星号(*)、减号(-)、底线(_)**</font>来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。

---

***

``````java
class test{
    public void add(){
        System.out.printf("add.....");
    }
}
``````

### 代码块

+ 单行代码用一对\`\`		***eg:***`示例代码`



+ 多行代码块用三对\`\`,需换行开始

  ***eg:***

```java
public interface test{
    public static void add();
}
```



+ 高亮强调用\=\=示例\=\=		***eg:*** 		==示例==



### Markdown语法支持html语言

**可以直接在 md 文档中嵌入 html语言**

```
<font color=red>需要改变字体格式的内容</font>
```

**color=red** 更改字体颜色（可以是 16进制也可以直接是颜色的英文）

**face=“宋体”** 更改字体

**size=4** 改变字体显示大小



***eg:***

```html
<table>
    <tr>
    	<th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
    	<td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>
```
<table>
    <tr>
    	<th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
    	<td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>                                   



### 引用：比如说引用了什么话之类的

在被引用的<font color=red face="宋体" size=3>**文本前加上 > 符号，以及一个空格**</font>就可以了，如果只输入了一个>符号会产生一个空白的引用。



**·单个的引用**



> 这是一个引用
>
> > 这还是一个引用
>
> 





**·多个引用**

> > 引用

> > > 引用

> 引用

> yinyong
>
> > yinyong
> >
> > > yinyong





### 列表

- 使用 <font color=red face="宋体" size=3>***，+，- 表示无序列表**</font>。

  注意：<font color=red face="宋体" size=3>**符号后面一定要有一个空格**</font>，起到缩进的作用。

***eg：***

- (\- )   列表
  + (\+ )  列表
    * (\* )  列表

- 使用<font color=red face="宋体" size=3>**数字和一个英文句点表示有序列表**</font>。
  注意：<font color=red face="宋体" size=3>**英文句点后面一定要有一个空格**</font>，起到缩进的作用。

  ***eg:***

  	1. 有序列表
  	2. 有序列表
  	3. 示范



### 表格

- 表格的基本写法跟表格的形状相识



学号|姓名|分数

\-\|\-\|\-

小明|男|75

小红|女|79

小陆|男|92



- 表格对齐方式：我们可以指定表格单元格的对齐方式，冒号在左边表示左对齐，右边表示有对齐，两边都有表示居中。

学号|姓名|分数

:-|:-:|-:

小明|男|75

小红|女|79

小陆|男|92





参考文章：<https://blog.csdn.net/u014061630/article/details/81359144?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164214252616780366594574%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164214252616780366594574&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-81359144.first_rank_v2_pc_rank_v29&utm_term=markdown%E8%AF%AD%E6%B3%95&spm=1018.2226.3001.4187>