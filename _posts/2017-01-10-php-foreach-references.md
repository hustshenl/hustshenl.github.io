---
layout: post
title:  "PHP 引用变量，以及 foreach 的引用变量导致的问题"
date:   2017-01-10 10:00:00
categories: php
tags: [php,foreach,引用变量]
excerpt: 从一道面试题引发的关于 PHP 的 foreach 循环中的引用变量的一些坑
---

* content
{:toc}

## 引言

本来是对 foreach 中临时变量的有一些疑惑，但是在 Google 的时候无意间看到了这道题。

## 问题

这是一道金山的面试题：

```php
$a = array('a','b','c');  
foreach($a as &$v){}
foreach($a as $v){}
var_dump($a);
```

看上去并没有什么复杂的地方，但是运行结果却并不是我以为的那样。

正确答案是：
`array(3) { [0]=> string(1) "a" [1]=> string(1) "b" [2]=> &string(1) "b"}`

## 分析

首先，在第一个循环里面打印数组：

```php
$a = array('a', 'b', 'c');
foreach ($a as &$v) {
    var_dump($a);
}
```

输出结果：

```php
array(3) { [0]=> &string(1) "a" [1]=> string(1) "b" [2]=> string(1) "c" }
array(3) { [0]=> string(1) "a" [1]=> &string(1) "b" [2]=> string(1) "c" }
array(3) { [0]=> string(1) "a" [1]=> string(1) "b" [2]=> &string(1) "c" }
```

可以看出 `foreach` 循环首先将数组 `$a` 的第一个元素设置为指向 `"a"` 的引用地址，将改引用地址赋值给 `$v` ，然后依此类推；

然后在第二个数组打印数组：

```php
$a = array('a', 'b', 'c');
foreach ($a as &$v) {
}
foreach ($a as $v) {
    var_dump($a);
}
```

输出结果：

```php
array(3) { [0]=> string(1) "a" [1]=> string(1) "b" [2]=> &string(1) "a" }
array(3) { [0]=> string(1) "a" [1]=> string(1) "b" [2]=> &string(1) "b" }
array(3) { [0]=> string(1) "a" [1]=> string(1) "b" [2]=> &string(1) "b" }
```

第二次循环中，`$v` 依然是指向上一次循环最后一个元素的地址的引用，此时，`foreach` 循环将个元素赋值给 `$v` 意味着将该值写入 `$v` 指向的实际地址，即同时改变了数组最后一个元素。

## 总结

1. 将变量赋值给引用变量是写入实际地址，改变实际地址的值；
2. 将变量地址赋值给引用变量是改变引用变量的指向；这两点和指针略有区别
3. 引用变量 `$v` 在作用域内始终指向某个地址，在第一轮循环完毕后始终指向 `$a[2]`的实际地址；
4. 问题中 `foreach` 循环的临时变量赋值正是第1、2条总结列出的情况。


编程时大部分情况会注意引用变量的赋值问题，但是跟 `foreach` 联合在一起时可能就会造成误解，特别是多次循环，临时变量名称重复的情况要特别注意。



