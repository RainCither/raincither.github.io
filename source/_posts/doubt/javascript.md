---
title: 【JavaScript】学习中遇到的一些小问题
date: 2020-06-14 16:08:58
description: 
hide: true
tags:
    - JavaScript
categories:
    - 雨筝的疑惑哎
---

### 时间格式化问题

 `Date` 的默认输出为：

```
Sun Jun 14 2020 16:01:55 GMT+0800 (中国标准时间)
```

可以为 `Date` 原型添加如下的方法：
```javascript
Date.prototype.format = function(fmt) { 
            var o = { 
                "M+" : this.getMonth()+1,                 //月份 
                "d+" : this.getDate(),                    //日 
                "H+" : this.getHours(),                   //小时 
                "m+" : this.getMinutes(),                 //分 
                "s+" : this.getSeconds(),                 //秒 
                "q+" : Math.floor((this.getMonth()+3)/3), //季度 
                "S"  : this.getMilliseconds()             //毫秒 
            }; 
            if(/(y+)/.test(fmt)) {
            fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length)); 
            }
            for(var k in o) {
                if(new RegExp("("+ k +")").test(fmt)){
                    fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));
                }
            }
            return fmt; 
        }       
```

测试：

```javascript
new Date.format("yyyy年MM月dd日 HH:mm:ss")
> 2020年06月14日 16:08:58
```