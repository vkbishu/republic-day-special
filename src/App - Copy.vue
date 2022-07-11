<template>
  <div id="app">
    <div class="red-fort" @click="startMusic" id="red-fort">
      <div class="flag-pos">
          <div class="flag">
              <img src="@/assets/flag-pole.png" alt="" class="pole" v-show="isStarted">
              <img src="@/assets/flag-h-2.gif" alt=""  v-show="isStarted" class="banner" :class="{'hoisting': isPlaying}">
            </div>
      </div>
      <img src="@/assets/redfort.webp" alt="" class="red-port-img">
      </div>
  </div>
</template>

<script>
let music_player;
import gsap from "gsap";
export default {
  name: 'App',
  data(){
    return {
      isStarted: false,
      isPlaying: false,
    }
  },
  created(){
    this.initialize();
  },
  methods: {
    initialize(){
      music_player = new Audio(require('@/assets/audio/republic-day.mp3'));
    },
    startMusic(){
      if(!this.isStarted){
          this.isStarted = true;
          console.log('Playing audio');
          setTimeout(() => {
            this.isPlaying = true;
            this.startFlowerFall();
          }, 1000);
          if(typeof music_player !== 'undefined'){
            music_player.play();
          }
      }
    },
    startFlowerFall(){
      gsap.set("#red-fort",{perspective:600});
      let total = 10;
      let container = document.getElementById("red-fort"),	w = window.innerWidth;
      for (let i=0; i<total; i++){
        let Div = document.createElement('div');
        let Div2 = document.createElement('div');
        let Div3 = document.createElement('div');
        gsap.set(Div,{attr:{class:'dot'},x:this.R(0,w),y:this.R(-200,-150),z:this.R(-200,200),xPercent:"-50%",yPercent:"-50%"});
        gsap.set(Div2,{attr:{class:'dot2'},x:this.R(0,w),y:this.R(-200,-150),z:this.R(-200,200),xPercent:"-50%",yPercent:"-50%"});
        gsap.set(Div3,{attr:{class:'dot3'},x:this.R(0,w),y:this.R(-200,-150),z:this.R(-200,200),xPercent:"-50%",yPercent:"-50%"});
        container.appendChild(Div);
        container.appendChild(Div2);
        container.appendChild(Div3);
        this.animm(Div);
        /* this.animm2(Div2);
        this.animm3(Div3); */
      }
    },
    animm(elm){
      console.log(elm);
      let h = window.innerHeight;
      gsap.to(elm,{duration: this.R(6,15), y:h+100,ease:"none",repeat:-1,delay:-15});
      gsap.to(elm,{duration: this.R(4,8), x:'+=100',rotationZ:this.R(0,180),repeat:-1,yoyo:true,ease:"sine"});
      gsap.to(elm,{duration: this.R(2,8), repeat:-1,yoyo:true,ease:"sine",delay:-5});
    },
    animm2(elm){
      let h = window.innerHeight;
      gsap.to(elm,{duration: this.R(6,15), y:h+100,ease:"none",repeat:-1,delay:-25});
      gsap.to(elm,{duration: this.R(4,8), x:'+=100',rotationZ:this.R(0,180),repeat:-1,yoyo:true,ease:"sine"});
      gsap.to(elm,{duration: this.R(2,8), repeat:-1,yoyo:true,ease:"sine",delay:-5});
    },
    animm3(elm){
      let h = window.innerHeight;
      gsap.to(elm,{duration: this.R(6,15), y:h+100,ease:"none",repeat:-1,delay:-5});
      gsap.to(elm,{duration: this.R(4,8), x:'+=100',rotationZ:this.R(0,180),repeat:-1,yoyo:true,ease:"sine"});
      gsap.to(elm,{duration: this.R(2,8), repeat:-1,yoyo:true,ease:"sine",delay:-5});
    },
    R(min,max) {
      return min+Math.random()*(max-min)
    }
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.flag-pos {
  position: relative;
  top: 100px;
}
.red-fort {
  position: fixed;
  bottom: 0;
  margin: auto;
  width: 100%;
  .red-port-img {
    max-width: 100%;
    position: relative;
    z-index: 99;
    top: 10px;
  }
}
.flag {
  position: relative;
  display: inline-block;
  top: 150px;
  width: 171px;
  .banner {
    position: absolute;
    top: 246px;
    left: 58px;
    width: 100%;
    transition: top 3s;
    &.hoisting {
      top: 3px;
    }
  }
  .pole {
    position: relative;
    z-index: 1;
  }
}

.dot{
  width:35px;
  height:35px;
  position:absolute;
  background: url(http://img.babathe.com/upload/specialDisplay/htmlImage/2019/20190104_rose2.png);
  background-size: 100% 100%;
}
.dot2{
  width:35px;
  height:35px;
  position:absolute;
  background: url(http://img.babathe.com/upload/specialDisplay/htmlImage/2019/20190104_rose.png);
  background-size: 100% 100%;
}
.dot3{
  width:35px;
  height:35px;
  position:absolute;
  background: url(http://img.babathe.com/upload/specialDisplay/htmlImage/2019/20190104_rose3.png);
  background-size: 100% 100%;
}
</style>
