# music-box-core

>Music box play
## Build Setup

插件使用方法
```
 npm i music-box-core
```
在main.js中引入
```
import MusicBox from 'music-box-core';

Vue.use(MusicBox);
```
在需要使用的.vue文件中直接使用组件
```
<music-box></music-box>即可
```

使用如下所示:
```
    <music-box :singList='list' :setVolume='getval' :repeatState="state" :setProgress='getprogress' @totalTime='totaltime' @currentTime='getime' ref='musicbox'></music-box>
```

>> 所有参数说明 <strong style='color:red'>重要参数</strong>
 ***
 
   singList:表示往组件传递一歌曲路径的数组,如['http://xxxxx','http://xxxx'],也可以只传递单首歌曲的路径，如'http://xxxx';
***
   setVolume:设置播放音量，可接受0-1之间的数值，0最小，1最大,默认值0.5
   ***
   repeatState:播放状态选择，例如，单曲循环，列表循环等。。。该状态有四个值'allCircle','currentCircle','playListOneTimesStop','randomPlay'，分别代表所有歌曲循环，当前歌曲循环，仅播放列表一次，随机播放
***
    setProgress:设置播放进度，进度范围0~[歌曲最大秒数]之间
***
    @totalTime:事件，组件传出的当前歌曲的最大秒数以及格式化为分秒的时间数
***
     @currentTime：组件传出的当前歌曲的播放进度秒数以及格式化为分秒的时间数
***
     @error:播放失败的错误事件，传出播放失败的一些信息
***
     @isPlay:是否播放状态，返回值为bolen类型
***
     @isNoVoice:是否静音状态,返回值为bolen类型
***
     @ended:当前歌曲是否播放完毕状态,返回值为bolen类型
***
     在<music-box ref='musicbox'></music-box>绑定ref钩子后，可操作组件
     可操作的方法有常见的播放，暂停，上一曲，下一曲，静音，
 播放以及暂停方法调用
 ```
 调用一次暂停，在调用一次恢复播放
 this.refs.musicbox.playing()
```
 上一曲以及下一曲调用
```
this.refs.musicbox.next();
this.refs.musicbox.last()
```
静音方法,调用一次静音，在调用一次恢复播放
```
this.refs.musicbox.novoice()

```
***
git Hub [docs for music-box-core](https://github.com/965390267/music--box).
