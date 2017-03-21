---
layout: post
title:  "vim cheet sheet"
date:   2017-03-14 03:25:50 +0900
categories: dev 
tags: dev info tool vim
---

# CHEET SHEET

### Regitster

* unnamed register 16개(순환적 0-9 + 특수 6개) 와 named register 26개(a-z)로 이루어져 있다. 

* How To use. For example, named register `a`
  
  * copy 
    > `"ay`
  
  * paste (insert mode)
    > `ctrl+r a`

  * paste (normal mode) 
    > `"ap`


### buffer

* buffer를 닫을때 윈도우가 같이 닫히기 때문에 매우 불편함. 이를 해결한 script - [Bclose](http://vim.wikia.com/wiki/Deleting_a_buffer_without_closing_the_window) 있음. 

