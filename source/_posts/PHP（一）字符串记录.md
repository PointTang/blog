---
title: PHP（一）字符串记录
date: 2018-07-31 16:37:47
tags: [PHP]
categories: [tip]
---
本篇记录PHP使用过程中用过的字符串处理方法。
<!-- more -->
PHP的String参考手册点[这里](http://www.w3school.com.cn/php/php_ref_string.asp "PHP 5 String函数")。
# 判断指定开头
PHP没有startWith方法，但是可以查下标来判断。
[strpos()](http://www.w3school.com.cn/php/func_string_strpos.asp "strpos()函数") 函数查找字符串在另一字符串中第一次出现的位置。
```
if(strpos('111字符串的开头是', '111' )===0){
	echo '字符串开头是111';
}
```
# 判断指定结尾
PHP同样没有endWith方法，但是可以用查找最后一次出现的位置来判断。
[strrchr()](http://www.w3school.com.cn/php/func_string_strrchr.asp "strrchr()函数")函数查找字符串在另一个字符串中最后一次出现的位置，并返回从该位置到字符串结尾的所有字符。
```
if(strrchr('字符串的结尾是222',"222")==".GC"){
	echo '字符串结尾是222';
}

```
# 字符串替换
PHP字符串替换使用str_replace方法。
[str_replace()](http://www.w3school.com.cn/php/func_string_str_replace.asp "str_replace()函数") 函数以其他字符替换字符串中的一些字符（区分大小写）。
[str_ireplace()](http://www.w3school.com.cn/php/func_string_str_ireplace.asp "str_ireplace()函数") 函数以其他字符替换字符串中的一些字符（不区分大小写）。
```
$str = str_replace ('A','B','把字符串里的A替换成B');
```
这个方法建议去看详情，我用的比较简单。

# 字符串去掉指定开头
PHP有ltrim方法去掉左侧的指定字符。不填第二个参数则去掉空格，不管有几个。
[ltrim()](http://www.w3school.com.cn/php/func_string_ltrim.asp "ltrim()函数") 函数移除字符串左侧的空白字符或其他预定义字符。
```
echo ltrim('接下来去掉这个字符串开头的接下来','接下来');
```

# 字符串去掉指定结尾
PHP有rtrim方法去掉右侧的指定字符。不填第二个参数则去掉空格，不管有几个。
[rtrim()](http://www.w3school.com.cn/php/func_string_rtrim.asp "rtrim()函数") 函数移除字符串右侧的空白字符或其他预定义字符。
```
echo rtrim('接下来去掉这个字符串结尾的接下来','接下来');
```

# 字符串去掉指定开头和结尾
PHP有trim方法去掉两侧的指定字符。不填第二个参数则去掉空格，不管有几个。
[trim()](http://www.w3school.com.cn/php/func_string_trim.asp "trim()函数") 函数移除字符串两侧的空白字符或其他预定义字符。
```
echo rtrim('接下来去掉这个字符串两侧的接下来','接下来');
```

