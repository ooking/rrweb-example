# rrweb 录制回放例子

官方文档：[rrweb/guide.zh_CN.md at master · rrweb-io/rrweb (github.com)](https://github.com/rrweb-io/rrweb/blob/master/guide.zh_CN.md)

为了方便看效果，写了个rrweb的example

录制方法，打开任意网站，然后在控制台输入以下代码后在网站上操作，20秒后就会输入json，把json复制出来存到文件中，然后改一下replay.html中的文件名即可看到效果。

```jsx
let injectJs= (url) => {
	let file = document.createElement("script");
  file.src= url;
  document.body.appendChild(file);
};

// https://www.jsdelivr.com/package/npm/rrweb
injectJs("https://cdn.jsdelivr.net/npm/rrweb@latest/dist/record/rrweb-record.min.js");

let time = 20 //录制多少秒
let events = [];
// 开始录制
rrwebRecord({
  emit(event) {
    events.push(event);
  },
	recordCanvas: true,
	recordCrossOriginIframes: true,
	// inlineImages: true,	
});

timer = setInterval(()=>{
	clearInterval(timer)
	rrwebRecord({emit(event){}});
	console.log(JSON.stringify({ events }));
  events = [];
}, time * 1000);

```
