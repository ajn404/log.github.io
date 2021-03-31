---
title: js懒加载
tags: 
 - 前端架构
---

### JS

```js
// This is "probably" IE9 compatible but will need some fallbacks for IE8
// - (event listeners, forEach loop)

// wait for the entire page to finish loading
window.addEventListener('load', function() {
	// setTimeout to simulate the delay from a real page load
	setTimeout(lazyLoad, 1000);	
});

function lazyLoad() {
	var card_images = document.querySelectorAll('.card-image');	
	// loop over each card image
    
	card_images.forEach(function(card_image) {
		var image_url = card_image.getAttribute('data-image-full');
		var content_image = card_image.querySelector('img');
		
		// change the src of the content image to load the new high res photo
		content_image.src = image_url;
		
		// listen for load event when the new photo is finished loading
		content_image.addEventListener('load', function() {
			// swap out the visible background image with the new fully downloaded photo
			card_image.style.backgroundImage = 'url(' + image_url + ')';
			// add a class to remove the blur filter to smoothly transition the image change
			card_image.className = card_image.className + ' is-loaded';
		});
	});	
}
```

### [Codepen](https://codepen.io/ajn404/pen/xxRBaPX)

以上为懒加载示例

### 分析

1. load:资源及相关资源已加载完成，[事件参考](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event)

2. 懒加载即延迟加载;当访问一个页面时,先将img标签中的src链接设为同一张图片(这样就只需请求一次,俗称占位图),将其真正的图片地址存储在img标签的自定义属性中(比如data-src);当js监听到该图片元素进入可视窗口时,即将自定义属性中的地址存储到src属性中,达到懒加载的效果;这样做能防止页面一次性向服务器响应大量请求导致服务器响应慢页面卡顿或崩溃等问题

3. ```css
   transition: transform 0.1s ease-in-out, box-shadow 0.1s;	
   &hover
   transform: translateY(-1.0rem) scale(1.0125);
   ```

   