---
title: 開始用blogdown寫blog啦
author: Yi-Ju Tseng
date: '2017-08-17'
slug: rblogdown
categories:
  - Blog
tags:
  - R
  - Blog
  - Packages
  - blogdown
Description: ''
menu: ''
---

忙一忙突然又想起blogdown這個packages，今天終於下定決心開始，不知道會持續多久.....

### blogdown

blogdown是基於R Markdown和Hugo開發的R package，簡單來說是讓你可以用R Markdown寫Blog的好幫手。先前用過姊妹套件[bookdown](https://bookdown.org/yihui/bookdown/)寫了[資料科學與R語言](yijutseng.github.io/DataScienceRBook/)電子書，用來介紹R語言的使用非常方便，不用再辛苦的剪下貼上，也不用擔心程式碼沒上色，導致不清不楚讀者沒耐心看(只有我沒耐心)。

### Hugo

決定要寫以後，花了大把時間挑主題(汗)，[Hugo](https://gohugo.io/)有蠻多不錯的[主題](https://themes.gohugo.io/)可選，因為希望頁面乾淨整潔，所以選了[hugo-theme-minos](https://github.com/carsonip/hugo-theme-minos)，希望不要馬上就看膩了。

卡關最久的是跟相對路徑有關的設定，不知道為什麼Tags跟Categories的連結一直設定不好，看起來是跟`$Site.BaseURL`有關，後來完全硬幹，把`$Site.BaseURL`全都改掉了，Google了老半天為什麼會這樣，到目前還是無解。

### Netlify

以往都是用GitHub page來存放靜態網頁，這次趁機會試著使用很多人推薦的[Netlify](https://www.netlify.com/)，一樣可以跟GitHub做連動，只要Commit & Sync，Netlify會自動部屬網頁，超級方便。
設定的時候有一些參數要注意:

```
Build command: hugo
Publish directory: public

Build environment variables
key: HUGO_VERSION   value:0.26
```

參考資料:

- [Making Websites with R Markdown and blogdown](https://slides.yihui.name/2017-rstudio-webinar-blogdown-Yihui-Xie.html )
- [blogdown GitHub page](https://github.com/rstudio/blogdown)