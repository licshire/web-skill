# 移动端常见问题 #
移动端开发常见问题汇总

## 目录
- [全屏视频实现方法](#全屏视频实现方法)
- [需要长按但不选中文本的方法](#需要长按但不选中文本的方法)
- [二维码在微信客户端长按识别注意事项](#二维码在微信客户端长按识别注意事项)
- [安卓输入框弹出键盘遮盖住文本框的解决办法](#安卓输入框弹出键盘遮盖住文本框的解决办法)

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
//css 不要用绝对定位
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
