 <template>
  <audio
    :src="musicsrc"
    @canplaythrough="loaded()"
    @timeupdate="timeupdate()"
    @ended="end()"
    ref="music"
  >您的浏览器版本过低，请升级您的浏览器</audio>

</template>

  
   <script>
export default {
  data() {
    return {
      currenttime: "", //已播放时间
      isplay: true, //是否播放状态
      isnovoice: false, //是否静音状态
      sing: [],
      musicsrc: "", //双向绑定音乐路径
      index: 0, //当前歌曲的索引,
    
     
    };
  },
  props: {
    singList:{
      type:Array,
      isValid: function(value) {
       return value.length>0
   }
    },
    setVolume:{
      type:null,
    },
    setProgress:null,
    initVolume:{
      type:Number,
      default:.3
    }
  },
  watch: {
    setVolume: function(val) {
      //音量君监听
    val=  typeof val=='string'?Number(val):val;
      this.$refs.music.volume = val / 100;
    },
    setProgress: function(val) {
      //设置播放进度
      val=  typeof val=='string'?Number(val):val; 
      this.$refs.music.currentTime = val;
    }
  },
  methods: {
    timeFormt(mm) {
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
      this.$refs.music.play();
      let formttime  = this.$refs.music.duration;
      this.$refs.music.volume =this.initVolume;
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
    timeupdate() {
      //当前歌曲已经播放的时间
      let formttime = (this.currenttime = this.$refs.music.currentTime);
      let allmm, allss, formtTime;

      this.$emit("currentTime", this.timeFormt(formttime));
    },
    end() {
      //当前歌曲是否播放完成处理

      if (this.$refs.music.ended) {
        this.index < this.sing.length - 1 ? this.index++ : (this.index = 0);

        this.musicsrc = this.sing[this.index];
      }
    },
    next() {
      //下一曲
      this.index < this.sing.length - 1 ? this.index++ : (this.index = 0);
      console.log(this.index);
      this.musicsrc = this.sing[this.index];
    },
    last() {
      //上一曲
      this.index > 0 ? this.index-- : (this.index = this.sing.length - 1);
      console.log(this.index);
      this.musicsrc = this.sing[this.index];
    }
  },
  mounted() {
    this.sing = this.singList;

    this.musicsrc = this.sing[this.index];

  }
};
</script>