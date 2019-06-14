 <template>
  <audio
    :src="musicsrc"
    @canplaythrough="loaded()"
    @timeupdate="timeupdate($event)"
    @ended="end()"
    @error="error()"
    ref="music"
  >您的浏览器版本过低，请升级您的浏览器!!</audio>
  <!-- 01 //错误状态 
02 Media.error; //null:正常 
03 Media.error.code; //1.用户终止 2.网络错误 3.解码错误 4.URL无效 
04 //网络状态 
05 Media.currentSrc; //返回当前资源的URL 
06 Media.src = value; //返回或设置当前资源的URL 
07 Media.canPlayType(type); //是否能播放某种格式的资源 
08 Media.networkState; //0.此元素未初始化 1.正常但没有使用网络 2.正在下载数据 3.没有找到资源 
09 Media.load(); //重新加载src指定的资源 
10 Media.buffered; //返回已缓冲区域，TimeRanges 
11 Media.preload; //none:不预载 metadata:预载资源信息 auto: 
12 //准备状态 
13 Media.readyState; //1:HAVE_NOTHING 2:HAVE_METADATA 3.HAVE_CURRENT_DATA 4.HAVE_FUTURE_DATA 5.HAVE_ENOUGH_DATA 
-------------------------------------------------------------------------------------------
|//0 = HAVE_NOTHING - 没有关于音频是否就绪的信息
|//1 = HAVE_METADATA - 关于音频就绪的元数据
|//2 = HAVE_CURRENT_DATA - 关于当前播放位置的数据是可用的，但没有足够的数据来播放下一帧/毫秒
|//3 = HAVE_FUTURE_DATA - 当前及至少下一帧的数据是可用的
|//4 = HAVE_ENOUGH_DATA - 可用数据足以开始播放
--------------------- -----------------------------------------------------------------------
   Media.error; //null:正常  
   Media.error.code; //1.用户终止 2.网络错误 3.解码错误 4.URL无效  
14 Media.seeking; //是否正在seeking 
15 //回放状态 
16 Media.currentTime = value; //当前播放的位置，赋值可改变位置 
17 Media.startTime; //一般为0，如果为流媒体或者不从0开始的资源，则不为0 
18 Media.duration; //当前资源长度 流返回无限 
19 Media.paused; //是否暂停 
20 Media.defaultPlaybackRate = value;//默认的回放速度，可以设置 
21 Media.playbackRate = value;//当前播放速度，设置后马上改变 
22 Media.played; //返回已经播放的区域，TimeRanges，关于此对象见下文 
23 Media.seekable; //返回可以seek的区域 TimeRanges 
24 Media.ended; //是否结束 
25 Media.autoPlay; //是否自动播放 
26 Media.loop; //是否循环播放 
27 Media.play(); //播放 
28 Media.pause(); //暂停 
29 //控制 
30 Media.controls;//是否有默认控制条 
31 Media.volume = value; //音量 
32 Media.muted = value; //静音 
33 //TimeRanges(区域)对象 
34 TimeRanges.length; //区域段数 
35 TimeRanges.start(index) //第index段区域的开始位置 
  36 TimeRanges.end(index) //第index段区域的结束位置-->
  <!-- audio事件： eventTester("loadstart"); //客户端开始请求数据 
 eventTester("progress"); //客户端正在请求数据 
 eventTester("suspend"); //延迟下载 该事件在媒体数据被阻止加载时触发。 可以是完成加载后触发，或者因为被暂停的原因。
 eventTester("abort"); //客户端主动终止下载（不是因为错误引起）， 
 eventTester("error"); //请求数据时遇到错误 
 eventTester("stalled"); //网速失速 
 eventTester("play"); //play()和autoplay开始播放时触发 
 eventTester("pause"); //pause()触发 
 eventTester("loadedmetadata"); //成功获取资源长度 当指定的音频/视频的元数据已加载时，会发生 loadedmetadata 事件。
 eventTester("loadeddata"); // 当当前帧的数据已加载，但没有足够的数据来播放指定音频/视频的下一帧时，会发生 loadeddata 事件
 eventTester("waiting"); //等待数据，并非错误 
 eventTester("playing"); //开始回放 
 eventTester("canplay"); //可以播放，但中途可能因为加载而暂停 
 eventTester("canplaythrough"); //可以播放，歌曲全部加载完毕 
 eventTester("seeking"); //寻找中 
 eventTester("seeked"); //寻找完毕 
 eventTester("timeupdate"); //播放时间改变 
 eventTester("ended"); //播放结束 
 eventTester("ratechange"); //播放速率改变 
 eventTester("durationchange"); //资源长度改变 
  eventTester("volumechange"); //音量改变-->
</template>

  
   <script>
export default {
  data() {
    return {
      isplay: true, //是否播放状态
      isnovoice: false, //是否静音状态
      sing: [],
      musicsrc: "", //双向绑定音乐路径
      index: 0, //当前歌曲的索引,
      repeatPlayFlag: "allCircle", //是否重复播放标志
      chooseRepeatIndex: 0
    };
  },
  props: {
    singList: {
      type: null,
      isValid: function(value) {
        return value.length > 0;
      }
    },
    setVolume: {
      type: null
    },
    setProgress: null,
    initVolume: {
      type: Number,
      default: 0.3
    },
    repeatState: {
      //歌曲循环状态，默认全部列表循环,可选项['currentCircle','randomPlay','allCircle','playListOneTimesStop'],当前歌曲重复，随机播放，列表循环,播放完一遍列表后停止
      type: String,
      default: "allCircle"
    },
    setRateChange: {
      type: Number,
      default: 1
    }
  },
  watch: {
    setVolume: function(val) {
      //音量君监听
      val = typeof val == "string" ? Number(val) : val;
      if (val > 1 || val < 0) {
        console.warn("音量设置最大值为1，最小为0,不合法将设置默认值0.5");
        val = 0.9;
      }
      this.$refs.music.volume = val;
    },
    setProgress: function(val) {
      //设置播放进度
      val = typeof val == "string" ? Number(val) : val;
      if (val > this.$refs.music.duration || val < 0) {
        console.warn("进度最大值不能超过歌曲最大值");
      }
      val = val > this.$refs.music.duration ? this.$refs.music.duration : val;
      val = val < 0 ? 0 : val;
      this.$refs.music.currentTime = val;
    },
    repeatState: function(val) {
      //根据传的值选择不同的重复类型，暂时有四种，分别是列表循环，单曲循环，随机播放，列表播放完毕一遍自动停止
      //单曲重复播放

      this.repeatPlayFlag = val;
      // this.chooseRepeatIndex=this.index;
    },
    setRateChange: function(val) {
      //设置播放速率
      this.$refs.music.playbackRate = val;
    }
  },
  methods: {
    error() {
      //错误时间监听
      console.warn("当前歌曲播放错误,自动播放下一曲");
      console.warn(
        "错误信息:" +
          this.$refs.music.error +
          "网络状态信息:" +
          this.$refs.music.networkState
      );
      this.next();
      var networkState = {
        "0": "NETWORK_EMPTY - 音频/视频尚未初始化",
        "1": "NETWORK_IDLE - 音频/视频是活动的且已选取资源，但并未使用网络",
        "2": " NETWORK_LOADING - 浏览器正在下载数据",
        "3": " NETWORK_NO_SOURCE - 未找到音频/视频来源"
      };
      var errState = {
        "1": "MEDIA_ERR_ABORTED - 取回过程被用户中止",
        "2": " MEDIA_ERR_NETWORK - 当下载时发生错误",
        "3": "MEDIA_ERR_DECODE - 当解码时发生错误",
        "4": "MEDIA_ERR_SRC_NOT_SUPPORTED - 不支持音频/视频"
      };
      this.$emit("error", {
        err: errState[this.$refs.music.error],
        netState: networkState[this.$refs.music.networkState]
      });
    },

    timeFormt(mm) {
      //时间格式化，提供初始的毫秒数和格式化为分，秒对象;
      let allmm,
        allss,
        initTime = mm;

      if (mm < 60) {
        //把毫秒数转换为分和杪
        mm = Math.round(mm);
        if (mm < 10) {
          //小于十秒前面加0
          mm = "0" + Math.round(mm);
        }
      } else {
        allmm = Math.floor(mm / 60);
        allss = Math.round(mm - allmm * 60);
        mm = allmm + ":" + allss;
      }

      return { mmTime: initTime, formtTime: mm };
    },
    loaded() {
      //歌曲加载完成可以播放后执行该函数

      //刚进入直接取父组件传过来的状态值，被改变以后watch才有作用
      if (this.repeatState == "randomPlay") {
        //随机播放
        this.musicsrc = this.sing[Math.floor(Math.random() * this.sing.length)];
      }
      this.$refs.music.play();
      let formttime = this.$refs.music.duration;
      this.$refs.music.volume = this.initVolume;
      this.$emit("totalTime", this.timeFormt(formttime));
    },
    playing() {
      //播放以及暂停功能实现
      if (this.isplay) {
        this.$refs.music.pause();
        this.isplay = false;
      } else {
        this.$refs.music.play();
        this.isplay = true;
      }
      this.$emit("isPlay", this.isplay);
    },
    novoice() {
      //静音功能实现
      this.isnovoice = !this.isnovoice;
      this.$refs.music.muted = this.isnovoice;
      this.$emit("isNoVoice", this.isnovoice);
    },
    timeupdate(e) {
      //当前歌曲已经播放的时间
      let formttime = e.target.currentTime;
      let allmm, allss, formtTime;
      //var timeRanges = e.target.buffered; // 获取已缓冲部分的 TimeRanges 对象
      // 获取以缓存的时间

      // var timeBuffered = timeRanges.end(timeRanges.length - 1);//会报错，不知道为啥(*╹▽╹*)
      this.$emit("currentTime", this.timeFormt(formttime));
    },
    end() {
      //当前歌曲是否播放完成处理

      if (this.$refs.music.ended) {
        this.$emit("ended", this.$refs.music.ended);
        this.$refs.music.loop = false;
        switch (this.repeatPlayFlag) {
          case "allCircle": //列表全循环
            this.index < this.sing.length - 1 ? this.index++ : (this.index = 0);
            this.musicsrc = this.sing[this.index];
            break;
          case "currentCircle": //当前歌曲循环
            this.$refs.music.loop = true;
            break;

          case "playListOneTimesStop": //播放一遍后停止
            if (this.index == this.sing.length - 1) {
              this.$refs.music.pause();
              this.index = 0;
            } else {
              this.musicsrc = this.sing[this.index++];
            }

            break;
          default:
            this.index < this.sing.length - 1 ? this.index++ : (this.index = 0);
            this.musicsrc = this.sing[this.index];
        }
      }
    },
    next() {
      //下一曲
      this.index < this.sing.length - 1 ? this.index++ : (this.index = 0);

      this.musicsrc = this.sing[this.index];
    },
    last() {
      //上一曲
      this.index > 0 ? this.index-- : (this.index = this.sing.length - 1);

      this.musicsrc = this.sing[this.index];
    }
  },
  mounted() {
    if (this.singList instanceof Array) {
      //传入的是否是数组，如果不是转换为只有一个路径的数组
      this.sing = this.singList;
      this.musicsrc = this.sing[this.index];
    } else if (typeof this.singList == "string") {
      this.sing = [this.singList];

      this.musicsrc = this.sing[this.index];
    } else {
      console.error("传入的歌曲列表必须为数组或歌曲路径字符串！");
    }
  }
};
</script>