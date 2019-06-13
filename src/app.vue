<template>
  <div id="app">
    <music-box :singList='list' :SaveVolume='getval' @currentTime='getime' ref='musicbox'></music-box>
    <input type='range' v-model="getval">
    <img src="./assets/logo.png">
    <button @click="next()">下一曲</button>
    <button @click="last()">上一曲</button>
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
      list:[
        'https://96.f.1ting.com/5d026c51/b1dd80884bb1101679d8c62bd39b3e1f/2019/03/25X/25a_Alu/11.mp3',
      'https://96.f.1ting.com/5d026c7b/84f5c5cd197d2b33c355bcff4c7889a1/zzzzzmp3/2009bFeb/06C/2/6.mp3',
      'https://96.f.1ting.com/5d026c99/9452f8505ca900db1f490a9a0f40e2cd/zzzzzmp3/2007CseP/10g_Dance6/4.mp3'

      ],//歌曲路径数组
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods:{
    next(){
      this.$refs.musicbox.next()
    },
     last(){
      this.$refs.musicbox.last()
    },
    getime(obj){
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
