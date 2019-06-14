<template>
  <div id="app">
    <music-box :singList='list' :setVolume='getval' :setProgress='getprogress' @totalTime='totaltime' @currentTime='getime' ref='musicbox'></music-box>
    <label>音量设置</label><input type='range' v-model="getval">
    <img src="./assets/logo.png">
    <button @click="next()">下一曲</button>
    <button @click="last()">上一曲</button>
      <button @click="play()">播放暂停</button>
       <button @click="novoice()">静音</button>
           <label>进度设置</label><input type='range' v-model="getprogress">
    <!-- <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul> -->
  </div>
</template>

<script>
 import music from './music'
export default {
  name: 'app',
  components:{
     MusicBox:music
  },
  data () {
    return {
      getval:0,
      getprogress:0,
      list:[
        'http://fs.open.kugou.com/2bb085fa30414868af2dd419e8b5213e/5d02f9a6/G131/M07/10/1D/Y5QEAFrLYY6ACcLBAEDyKN1O2fg382.mp3',
      'http://fs.open.kugou.com/3b671c3055895a9fe3454107bf18f846/5d02f9c2/G012/M02/0A/11/TA0DAFUJxG2ANyJpAD1wRLKAb54110.mp3',
      'http://fs.open.kugou.com/4dcb5335fd7dbce10de341bc33fe4ff0/5d02fa09/G002/M05/05/1F/ooYBAFS7CpKAWXV5ADq0ej59KvI332.mp3'

      ],//歌曲路径数组
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods:{
    novoice(){
 this.$refs.musicbox.novoice()
    },
    play(){
 this.$refs.musicbox.playing()
    },
    next(){
      this.$refs.musicbox.next()
    },
     last(){
      this.$refs.musicbox.last()
    },
    getime(obj){
       console.log(obj) 
    },
    totaltime(obj){
      console.log(obj);
      
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
