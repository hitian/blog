---
aliases:
- /develope/2014/04/02/cocos2d-x-touch-layer-block/
categories:
- develope
category_backup: develope
date: "2014-04-02T00:00:00Z"
description: cocos2d-x2.2.2 触摸事件的遮挡
tags:
- touch
- cocos2d-x
title: cocos2d-x2.2.2 触摸事件的遮挡
---

环境 cocos2d-x 2.2.2 + js

例如一个这样的界面

[![image](/assets/images/201404/2014-04-02-cocos2dx-layer-block.png "image")](/assets/images/201404/2014-04-02-cocos2dx-layer-block.png)

左上角的设置使用 cc.MenuItemSprite, 弹出的遮罩层和上面的按钮需要优先获取touch事件, 并阻止向后继续传播.

这里需要两个点就可以做到.

```javascript
var bgLayer = cc.LayerColor.extend({
    ctor: function () {
        this._super();
        cc.associateWithNative(this, cc.LayerColor);
        cc.registerTargetedDelegate(-129, true, this);  //1.设置触摸的优先级
        this.setTouchEnabled(true);
    },
    onTouchBegan: function (touches, event) {
        cc.log("TouchBegan: ");
        return true; //2. 在onTouchXXX 回调里返回true来吞噬掉touch事件
    },
    onTouchesBegan: function (touches, event) {
        var loc = touches[0].getLocation();
        cc.log("TouchesBegan: " + loc.x + " : " + loc.y);
        return true;
    }
});
```

### 1.设置触摸的优先级 ###

在layer创建的时候使用 `cc.registerTargetedDelegate(-129, true, this);` 来设置优先级(第1个参数)

这里的数字越小, 优先级越高,

另外要注意的是 MenuItem 的优先级是` -128 `, 所以要遮挡上上面的设置按钮, 遮盖层的优先级必须小于它.

### 2.在onTouchXXX 回调里返回true来吞噬掉touch事件. ###

这里注意不是 onTouchesXXX, 看了下c++的说明, 因为多点的触摸使用的StandardDelegate并没有吃掉该事件的相关参数, 所以在onTouchesXXX 里 `return true` 是没有用的.



PS 之前一直用的onTouchesXXX 来处理触摸的, 没发现居然不一样, 这个问题折腾了2个多小时, api里没有, 网上查到的各种说明都是c++的. 坑啊...


