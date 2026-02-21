---
title: 学习日志-Java-01
tags: Java
summary: >-
  这里是言寺AI，这篇文章是一篇关于Java学习日志Day01的内容，主要介绍了Java的8大基本数据类型包括char、byte、short、int、long、float、double和boolean，并提到可以通过Integer.MAX_VALUE和Integer.MIN_VALUE等方法获取这些数据类型的值域，同时讲解了Java中的自动类型转换顺序为byte到double，且仅适用于小类型转大类型，大类型转小类型需要强制类型转换，特别指出byte、short转char也需要强制转换，此外还列举了Java中的运算符类型，包括算数运算符、关系运算符、逻辑运算符、赋值运算符以及它们的优先级，整体内容聚焦于Java基础数据类型与运算符的基础知识与使用注意事项。
date: 2025-09-15 14:46:30
---

# 学习日志——Java——day01

> 2025.09.15

## 数据类型

在Java中，有着8大基本数据类型

<table>
  	<tr>
		<td></td>
		<td>类型名称</td>
    </tr>
	<tr>
		<td rowspan="8" width="300px">8大基本数据类型</td>
        <td>char</td>
    </tr>
    <tr>
        <td>byte</td>
    </tr>
    <tr>
    	<td>short</td>
    </tr>
    <tr>
    	<td>int</td>
    </tr>
    <tr>
    	<td>long</td>
    </tr>
    <tr>
    	<td>float</td>
    </tr>
    <tr>
    	<td>double</td>
    </tr>
    <tr>
    	<td>boolean</td>
    </tr>
</table> 

可以通过以下代码获取该类型的值域

```java
System.out.print(Integer.MAX_VALUE)
System.out.print(Integer.MIN_VALUE)
```

在数据类型中，自动类型转换顺序为：byte->short->int->long->float->double。自动类型转换只使用于从小变成大中。如果要将大的变做小的则需要通过强制类型转换。

注意：byte、short->char需要通过强制类型转换。

## 运算符

<table>
	<tr>
    	<td>算数运算符</td>
    </tr>
    <tr>
    <td>关系运算符</td>
    </tr>

        
    <td>逻辑运算符</td>
        <td>赋值运算符</td>
        <td>优先级</td>
    </tr>
</table>