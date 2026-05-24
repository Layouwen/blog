---
title: HBuilderX 篇
date: 2024-01-23T15:22:00.000Z
tags:
  - 汇总
  - HBuilderX
categories:
  - 汇总
  - 开发环境
uuid: e68ccd31-f415-46fb-9fc9-92fa2b14fc68
---

# 配置默认终端

编辑 **HBuilderX** 根目录下的 `HBuilderX\plugins\builtincef3terminal\script\main.js` 文件

```js
var shell
if (isWin) {
  shell = 'D:\\cmder\\vendor\\git-for-windows\\bin\\bash.exe'
  // shell = 'powershell.exe'
  // var osRelease = os.release();
  // var dotIndex = osRelease.indexOf('.');
  // if(dotIndex>0){
  // 	var fv = osRelease.substring(0,dotIndex);
  // 	if(fv>6){
  // 		shell = 'powershell.exe';
  // 	}else{
  // 		shell = 'cmd.exe';
  // 		var ov = osRelease.substring(dotIndex);
  // 		dotIndex = ov.indexOf('.');
  // 		if(dotIndex>0){
  // 			var sv = ov.substring(0,dotIndex);
  // 			if(sv>1){
  // 				shell = 'powershell.exe';
  // 			}
  // 		}
  // 	}
  // }
} else {
  shell = 'bash'
}
```
