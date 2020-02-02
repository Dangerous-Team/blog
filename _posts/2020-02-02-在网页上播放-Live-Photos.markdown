---
layout: post
title:  "在网页上播放 Live Photos"
author: "彭淜"
date:   2020-02-02 13:45:00 +0800
lang:   "zh-CN"
categories: 
    - Web
    - JavaScript
---
<script src="/blog/assets/js/livephotoskit/livephotoskit.js"></script>
<div id="livephoto" style="width: 320px; height: 240px; margin:0px auto 15px"></div>
<script>
'use strict';
const player = LivePhotosKit.augmentElementAsPlayer(document.getElementById('livephoto'));
player.photoSrc = '/blog/assets/file/2020-02-02-fireworks.jpg';
player.videoSrc = '/blog/assets/file/2020-02-02-fireworks.mov';
player.addEventListener('canplay', evt => console.log('player ready', evt));
player.addEventListener('error', evt => console.log('player load error', evt));
player.addEventListener('ended', evt => console.log('player finished playing through', evt));
</script>

Live Photo (中文名: 实况照片) 是 iOS 系统相机的一个特性[^1]，它在拍照的时候可以同时摄入段 3s 的小视频。

拍摄的 Live Photo 主要也是由两个部分组成，HEVC[^2] 格式的 MOV 视频和 HEIF[^3] 格式的 HEIC 图片。

1.引入 Apple 官方提供的 LivePhotosKit JS 插件[^4]。

```
<script src="https://cdn.apple-livephotoskit.com/lpk/1/livephotoskit.js"></script>
```

2.开启 JavaScript 严格模式。

``` javascript
'use strict'
```

3.添加 HTML 标签。

``` html
<div
    data-live-photo
    data-photo-src="https://..."
    data-video-src="https://..."
    style="width: 320px; height: 320px">            
</div>
<!-- Player是零宽高的，所以务必请定义 div 的宽和高 -->
```
> 图片和视频一定一定一定要放在跟网页同一的服务容器内不然你将得到 `No 'Access-Control-Allow-Origin' header` 跨域警告

#### 🍺Done！你现在就能看到 Live Photo 显示在你的网页上了。

当然，你也可以使用 JavaScript 调用提供的 Api 显示 Live Photo，这样可以由更多的自定义选择。



[^1]: 支持的机型: iPhone 6s 或更新机型、iPad (第 5 代) 或更新机型、iPad Air (第 3 代)、iPad mini (第 5 代) 和 iPad Pro (所有机型)。
[^2]: HEVC: High Efficiency Video Coding，是一种视频压缩标准，用来以替代H.264/AVC编码标准，在2013年1月26号，HEVC正式成为国际标准。
[^3]: HEIF: High Efficiency Image Format，是一种图像容器格式，它基于 HEVC 技术，通过使用更先进的压缩算法来实现图片的压缩存储。
[^4]: LivePhotosKit JS: https://developer.apple.com/documentation/livephotoskitjs