# HTML5->video
##<a id="0">目录†</a>
- <a href="#1">video使用</a>
- <a href="#2">属性</a>
- <a href="#3">支持的格式</a>
- <a href="#5">HTML 音频/视频 方法</a>
- HTML 音频/视频事件(查看文档)
- <a href="#4">用时遇到的问题</a>
## <a id="1">video使用</a><a href="#0">†</a>
~~~
<video width="320" height="240" controls>
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.ogg" type="video/ogg">
    您的浏览器不支持 video 标签。
</video>
~~~
## <a id="2">属性</a><a href="#0">†</a>
属性	|值	|描述
:--:|:--:|:--:
autoplay|autoplay	|如果出现该属性，则视频在就绪后马上播放。
controls|controls	|如果出现该属性，则向用户显示控件，比如播放按钮。
height	|pixels	|设置视频播放器的高度。
loop	|loop	|如果出现该属性，则当媒介文件完成播放后再次开始播放。
muted	|muted	|如果出现该属性，视频的音频输出为静音。
poster	|URL	|规定视频正在下载时显示的图像，直到用户点击播放按钮。
preload	|auto metadata none	|如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。
src|	URL|	要播放的视频的 URL。
width|	pixels|	设置视频播放器的宽度。
##<a id="3">支持的格式</a><a href="#0">†</a>
格式|	MIME-type
:--:|:--:
MP4	|video/mp4
WebM|	video/webm
Ogg	|video/ogg
## <a id="5">HTML 音频/视频 方法</a><a href="#0">†</a>
方法|	描述
:--:|:--:
addTextTrack()|	向音频/视频添加新的文本轨道。
canPlayType()	|检测浏览器是否能播放指定的音频/视频类型。
load()|	重新加载音频/视频元素。
play()	|开始播放音频/视频。
pause()|	暂停当前播放的音频/视频。
## <a id="6">HTML 音频/视频事件</a><a href="#0">†</a>

## <a id="4">使用时遇到的问题</a><a href="#0">†</a>
1. 谷歌浏览器设置autoplay不能自动播放
~~~
//解决：添加muted="muted"
<video autoplay="autoplay" loop="loop" muted="muted">
	<source src="resources/background/Twinkling%20Stars%20Animation.mp4"  type="video/mp4">
</video>
~~~