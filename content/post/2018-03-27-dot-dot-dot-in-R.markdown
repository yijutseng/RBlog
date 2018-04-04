---
title: R的arguments出現三個點是...?
author: Yi-Ju Tseng
date: '2018-03-27'
slug: dotdotdot
categories: [R language]
tags:
  - R
---

## R函數介紹

需要先介紹怎麼在R裡面寫函數

```r
f <- function(...) { 
  names(list(...)) 
} 
```
上述程式碼中，·`f`是一個使用者自訂函數，就像java裡面的method，而**…**的功能，則是讓`f`函數可以輸入各式各樣的參數，而且可以將參數往下丟到其他函數內。

## ...使用範例一

舉例來說，若執行`f(a = 1, b = 2)`，a和b這兩個參數可以直接傳到`list()`函數中，變成`names(list(a=1,b=2))`，意即生成一個list，內有兩個元素，a=1以及b=2。

```r
f(a = 1, b = 2)
```

```
## [1] "a" "b"
```

```r
names(list(a=1,b=2)) #輸出相同
```

```
## [1] "a" "b"
```

## ...使用範例二


```r
addPercent <- function(x, mult = 100, ...){ 
  percent <- round(x * mult, ...) 
  paste0(percent, "%") 
}
```

在上述程式碼中，`addPercent`是一個使用者自訂函數，**…**的功能讓`addPercent`函數可以輸入各式各樣的參數，而且可以將參數往下丟到其他函數內。

```r
addPercent(c(0.5887,0.6925), digits = 2)
```

```
## [1] "58.87%" "69.25%"
```

```r
addPercent(c(0.5887,0.6925), digits = 1)
```

```
## [1] "58.9%" "69.2%"
```
因此在執行`addPercent(c(0.5887,0.6925), digits = 2)`時，`digits`參數被傳遞到自定義函數`addPercent`中的`round`函數內，變成`round`函數中的參數。
在`round`函數中，`digits`參數的功能是設定四捨五入的位數，因此`digits = 2`是指四捨五入到小數點第二位，`digits = 1`是指四捨五入到小數點第一位。


## ...使用範例三

```r
HelloWorld <- function(...) {
  arguments <- list(...) 
  paste(arguments)
} 
```

```r
HelloWorld("Hello", "World", "!","~")
```

```
## [1] "Hello" "World" "!"     "~"
```
