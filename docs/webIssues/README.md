# 移动端常见问题 #
移动端开发常见问题汇总

## 目录
- [全屏视频实现方法](#全屏视频实现方法)
- [需要长按但不选中文本的方法](#需要长按但不选中文本的方法)
- [二维码在微信客户端长按识别注意事项](#二维码在微信客户端长按识别注意事项)
- [安卓输入框弹出键盘遮盖住文本框的解决办法](#安卓输入框弹出键盘遮盖住文本框的解决办法)
- [安卓全屏视频无播放条高度保持不变解决办法](#安卓全屏视频无播放条高度保持不变解决办法)

### 全屏视频实现方法

```html
//HTML
<div class="video-box">
	<video class="action" poster="" src="" preload="auto" width="640" height="1030" x-webkit-airplay="true" playsinline="true" webkit-playsinline="true"></video>
</div>
```

```css
//css
.video-box {
	width: 100%;
	height: 100%;
	position: absolute;
	top: 0;
	left: 0;
	background: none no-repeat center; 
}
.video-box video {
    position: absolute
}
```

```javascript
//js
function eResize(e){
	var cw = 640,
	ch = document.documentElement.clientHeight,
		vScale, vwScale, vhScale;
	vwScale = cw / 640, vhScale = ch / 1030;
	vScale = vwScale > vhScale ? vwScale : vhScale;
	$(e).css({
		'-webkit-transform': 'scale(' + vScale + ')',
		'-webkit-transform-origin': 'center top'
	});
} 
eResize('.video-box');
$(window).bind("resize",function(){
	eResize('.video-box');
});
```


### 需要长按但不选中文本的方法

```css
//css
-webkit-touch-callout: none; /* disables the callout */
-webkit-user-select:none;
```

### 二维码在微信客户端长按识别注意事项

```html
//html 用img标签
<img class="qr" src="img/s4/qr.png" alt="">
```

//css 不要用绝对定位 定位时只能用padding
```css
.qr {
    padding: 327px 0 0 144px;
}
```

### 安卓输入框弹出键盘遮盖住文本框的解决办法

```javascript
if(/Android [4-6]/.test(navigator.appVersion)) {
	window.addEventListener("resize", function() {
		if(document.activeElement.tagName=="INPUT" || document.activeElement.tagName=="TEXTAREA") {
			window.setTimeout(function() {
				document.activeElement.scrollIntoViewIfNeeded();
			},0);
		}
	})
}
```

### 安卓全屏视频无播放条高度保持不变解决办法

安卓微信默认与使用X5同层对比图
![安卓微信默认与使用X5同层对比图](../../images/fullScene.jpg)

Demo： [点此查看](http://test.go.163.com/go/2015/public/team/ningbo/geyoutaidu/test.html)  
原文链接： [点此查看](https://zhuanlan.zhihu.com/p/27559167)

```html
<meta name="viewport" content="width=640,target-densitydpi=device-dpi,user-scalable=no">
<body>
  <header id="header" class="header"></header>
  <video id="video" class="video" poster="img/bg.jpg" src="http://flv2.bn.netease.com/videolib3/1707/31/UwslJ1623/HD/UwslJ1623-mobile.mp4" width="640" preload="auto" x-webkit-airplay="true" playsinline="true" webkit-playsinline="true" x5-video-player-type="h5" x5-video-player-fullscreen="true"></video>
</body>
```

```css
.video {
  position: absolute;
}
.fullscreen .video {
  width: 100%;
  height: 100%;
  object-position: center 128px;
}
.fullscreen .header {
  width: 100%;
  height: 128px;
  background: #373B3E;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 9999;
}
```

```javascript
var player = document.getElementById('video');

player.addEventListener('x5videoenterfullscreen', function() {
  // 设为屏幕尺寸
  player.style.width = document.body.width + 'px';
  player.style.height = (document.body.height-128) + 'px';
  // 在body上添加样式类以控制全屏下的展示
  document.body.classList.add('fullscreen');
});

player.addEventListener('x5videoexitfullscreen', function() {
  player.style.width = player.style.height = '';
  document.body.classList.remove('fullscreen');
}, false);
```