# rrweb
官方文档：[rrweb/guide.zh_CN.md at master · rrweb-io/rrweb (github.com)](https://github.com/rrweb-io/rrweb/blob/master/guide.zh_CN.md)

为了方便看效果，写了个rrweb的example
通过在控制台执行下面代码 ，可以把录制任意的网页内容，只要把输出的json复制成文件然后改一下replay.html中的文件名即可看到效果。

例子：

录制

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

回放页面

```jsx
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8" />
  <title>Player Example</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/rrweb-player@latest/dist/style.css" />
  <script src="https://cdn.jsdelivr.net/npm/rrweb-player@latest/dist/index.js"></script>
</head>
<body>
    <script>
        var url = "./ql.events.json"
        fetch(url, {cache: "no-cache"}).then(response => {
            return response.json();
        }).then(data => {
            new rrwebPlayer({
                target: document.body,                
                props: {
                    events: data.events,
                    autoPlay: true,
                    UNSAFE_replayCanvas: true
                },
            });
        }).catch(err => {
            console.log(err);
        });
    </script>
</body>
</html>
```
