---
aliases:
- /notes/2016/03/13/SourceTree-crash-by-font-osx/
categories: notes
date: "2016-03-13T00:00:00Z"
tags:
- OSX
- SourceTree
- Font Book
- crash
title: OSX上不正确的字体设置导致部分软件崩溃的问题
---

SourceTree 和 FontBook 打开就崩溃有一段时间了. 部分crash log 如下;

```bash
Process:               SourceTree [555]
Path:                  /Applications/SourceTree.app/Contents/MacOS/SourceTree
Identifier:            com.torusknot.SourceTreeNotMAS
Version:               2.2.2 (51)
Code Type:             X86-64 (Native)
Parent Process:        ??? [1]
Responsible:           SourceTree [555]
User ID:               501

Date/Time:             2016-03-13 15:34:07.628 +0800
OS Version:            Mac OS X 10.11.3 (15D21)
Report Version:        11


Time Awake Since Boot: 550 seconds

System Integrity Protection: enabled

Crashed Thread:        15  Dispatch queue: com.apple.root.default-qos

Exception Type:        EXC_BAD_ACCESS (SIGSEGV)
Exception Codes:       KERN_INVALID_ADDRESS at 0x000007fc4a1f7ab0
Exception Note:        EXC_CORPSE_NOTIFY

VM Regions Near 0x7fc4a1f7ab0:
    Process Corpse Info    0000000128acb000-0000000128ccb000 [ 2048K] rw-/rwx SM=COW  
--> 
    JS JIT generated code  00003a1034000000-00003a1034001000 [    4K] ---/rwx SM=NUL  

Application Specific Information:
objc_msgSend() selector name: retain


Thread 0:: Dispatch queue: com.apple.main-thread
0   com.apple.Foundation          	0x000000010c558949 -[_NSCachedIndexSet autorelease] + 8
1   com.apple.AppKit              	0x000000010d09762c -[NSTableView(NSTableViewViewBased) hiddenRowIndexes] + 56
2   com.apple.AppKit              	0x000000010d0974ea -[_NSTableRowHeightStorage _normalComputeRectOfRow:assumingExists:] + 151
3   com.apple.AppKit              	0x000000010cfcbf46 -[_NSTableRowHeightStorage computeRectOfRow:assumingExists:] + 167
4   com.apple.AppKit              	0x000000010cfcbe5c -[NSTableView _rectOfRowAssumingRowExists:] + 178
5   com.apple.AppKit              	0x000000010cfcbb80 -[NSTableView rectOfRow:] + 88
6   com.apple.AppKit              	0x000000010cfd5e40 -[NSTableView frameOfCellAtColumn:row:] + 308
7   com.torusknot.SourceTreeNotMAS	0x000000010a45922e 0x10a330000 + 1217070
8   com.apple.AppKit              	0x000000010d09a1f3 -[NSTableView drawRow:clipRect:] + 1144
9   com.apple.AppKit              	0x000000010d099aeb -[NSTableView drawRowIndexes:clipRect:] + 919
10  com.apple.AppKit              	0x000000010d098403 -[NSTableView drawRect:] + 1480
11  com.apple.AppKit              	0x000000010cf311fe -[NSView(NSInternal) _recursive:displayRectIgnoringOpacity:inGraphicsContext:CGContext:topView:shouldChangeFontReferenceColor:] + 1331
12  com.apple.AppKit              	0x000000010cf30b98 __46-[NSView(NSLayerKitGlue) drawLayer:inContext:]_block_invoke + 242
13  com.apple.AppKit              	0x000000010cf30843 -[NSView(NSLayerKitGlue) _drawViewBackingLayer:inContext:drawingHandler:] + 2403
14  com.apple.AppKit              	0x000000010cf2fed5 -[NSView(NSLayerKitGlue) drawLayer:inContext:] + 108
15  com.apple.AppKit              	0x000000010d0d0d8c -[_NSBackingLayerContents drawLayer:inContext:] + 157
16  com.apple.QuartzCore          	0x000000010f6d3927 -[CALayer drawInContext:] + 257
17  com.apple.AppKit              	0x000000010d0d0862 -[_NSTiledLayer drawTile:inContext:] + 625
18  com.apple.AppKit              	0x000000010d0d0593 -[_NSTiledLayerContents drawLayer:inContext:] + 185
19  com.apple.QuartzCore          	0x000000010f6d3927 -[CALayer drawInContext:] + 257
20  com.apple.AppKit              	0x000000010d0d04d1 -[NSTileLayer drawInContext:] + 169
21  com.apple.QuartzCore          	0x000000010f6d1e79 CABackingStoreUpdate_ + 3494
22  com.apple.QuartzCore          	0x000000010f6d10cd ___ZN2CA5Layer8display_Ev_block_invoke + 59
23  com.apple.QuartzCore          	0x000000010f6c4d31 CA::Layer::display_() + 1565
24  com.apple.AppKit              	0x000000010d0d03e8 -[NSTileLayer display] + 119
25  com.apple.AppKit              	0x000000010d850b34 -[_NSTiledLayerContents update:shouldCallPrepareContent:] + 7131
26  com.apple.AppKit              	0x000000010cf7aca4 -[_NSTiledLayer display] + 368
27  com.apple.QuartzCore          	0x000000010f6c310d CA::Layer::display_if_needed(CA::Transaction*) + 603
28  com.apple.QuartzCore          	0x000000010f6c278d CA::Layer::layout_and_display_if_needed(CA::Transaction*) + 35
29  com.apple.QuartzCore          	0x000000010f6c1cf1 CA::Context::commit_transaction(CA::Transaction*) + 277
30  com.apple.QuartzCore          	0x000000010f6c1a24 CA::Transaction::commit() + 508
31  com.apple.QuartzCore          	0x000000010f6d0917 CA::Transaction::observer_callback(__CFRunLoopObserver*, unsigned long, void*) + 71
32  com.apple.CoreFoundation      	0x000000010b469e37 __CFRUNLOOP_IS_CALLING_OUT_TO_AN_OBSERVER_CALLBACK_FUNCTION__ + 23
33  com.apple.CoreFoundation      	0x000000010b469da7 __CFRunLoopDoObservers + 391
34  com.apple.CoreFoundation      	0x000000010b45b358 CFRunLoopRunSpecific + 328
35  com.apple.HIToolbox           	0x00000001101cf935 RunCurrentEventLoopInMode + 235
36  com.apple.HIToolbox           	0x00000001101cf76f ReceiveNextEventCommon + 432
37  com.apple.HIToolbox           	0x00000001101cf5af _BlockUntilNextEventMatchingListInModeWithFilter + 71
38  com.apple.AppKit              	0x000000010cecd0ee _DPSNextEvent + 1067
39  com.apple.AppKit              	0x000000010d299943 -[NSApplication _nextEventMatchingEventMask:untilDate:inMode:dequeue:] + 454
40  com.apple.AppKit              	0x000000010cec2fc8 -[NSApplication run] + 682
41  com.apple.AppKit              	0x000000010ce45520 NSApplicationMain + 1176
42  libdyld.dylib                 	0x0000000111d135ad start + 1
```

可以看出来应该是字体导致的问题, 尝试了网上的大部分解决方案, 包括 清理 `Library/FontCollections`, 重建缓存什么的, 都没有效果.

但是在GUEST账户下, 并没有崩溃的问题, 猜测和当前用户的某些配置有问题.

终端执行 `defaults read > ~/Downloads/defaults.txt` 导出当前用户的配置信息.

全局搜索`NSFont`, 在 `Apple Global Domain` 节点下发现了 `NSFont = MicrosoftYaHei;`

看起来是之前测试MS Office 留下的. 卸载之后估计 MicrosoftYaHei 也没有了.

打开 `TinkerTool` >> `Fonts` 使用 `Set to default` 之后崩溃的问题全部解决了.

