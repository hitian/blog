---
aliases:
- /js/2011/08/24/js-tools/
categories:
- js
category_backup: js
date: "2011-08-24T00:00:00Z"
tags:
- js
title: js常用操作
---
```js
/**  
 * js常用操作类  
 */ 
var myTools = window.myTools = {   
    // getElementById   
    g: function(id){   
        return document.getElementById(id);   
    },   
    // getElementsByTagName   
    t: function(name){   
        return document.getElementsByTagName(name);   
    },   
    writeHTML: function(id, content){   
        var target = this.g(id);   
        if (target != null) {   
            target.innerHTML = content;   
        }   
    },   
    //计算字符串长度， 中文算两个字   
    slen: function(str){   
        c = 0;   
        for (var i = 0; i < str.length; i++)    
            (str.charCodeAt(i) > 255) ? c += 2 : c++;   
        return c;   
    },   
    // 还原html   
    HtmlToStr: function(vStr){   
        vStr = vStr.replace(/&amp;/g, "&");   
        vStr = vStr.replace(/&quot;/g, '"');
        vStr = vStr.replace(/&#039;/g, "'");  
        vStr = vStr.replace(/&lt;/g, "<");  
        vStr = vStr.replace(/&gt;/g, ">");  
        return vStr;  
    },  
    // 转译html  
    StrToHtml: function(str){  
        str = str.replace("&", /&amp;/g);  
        str = str.replace('"', /&quot;/g);   
        str = str.replace("'", /&#039;/g);   
        str = str.replace("<", /&lt;/g);   
        str = str.replace(">", /&gt;/g);   
        return str;   
    },   
    //按长度截取中文字符串   
    subs: function(str, len){   
        var l = 0, s = "";   
        for (var i = 0; i < str.length; i++) {   
            (str.charCodeAt(i) > 128) ? l += 2 : l++;   
            s += str.charAt(i);   
            if (l >= len) {   
                return s;   
            }   
        }   
        return s;   
    },   
    //获取httprequest对象   
    GetHttpRequest: function(){   
        if (window.XMLHttpRequest) { // Gecko   
            return new XMLHttpRequest();   
        }   
        else {   
            if (window.ActiveXObject) { // IE   
                return new ActiveXObject("MsXml2.XmlHttp");   
            }   
        }   
    },   
    load_js_url: '',   
    //加载js   
    loadJs: function(_url){   
        var callback = arguments[1] ||   
        function(){   
        };   
        this.load_js_url = _url;   
        var _script = $c("SCRIPT");   
        _script.setAttribute("type", "text/javascript");   
        _script.setAttribute("src", _url);   
        $t("head")[0].appendChild(_script);   
        if (document.all) {   
            _script.onreadystatechange = function(){   
                if (/onload|loaded|complete/.test(_script.readyState)) {   
                    callback && callback();   
                }   
            };   
        }   
        else {   
            _script.onload = function(){   
                callback();   
            };   
        }   
    },   
    //去空格   
    trim: function(str){   
        if (str != null && str.length >= 1) {   
            return str.replace(/(^\s*)/g, "");   
        }   
    }   
};
```

