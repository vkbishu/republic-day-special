<template>
  <div id="app">
      <h1 class="name" v-if="isStarted">Happy Republic Day <br/> {{name}}</h1>
      <div class="bottom-fixed centered">
        <div class="click-here-btn" @click="startMusic" v-if="!isStarted">Click Here</div>
        <div class="click-here-btn" v-if="isStarted" @click.prevent="share">Share</div>
      </div>
    <div class="red-fort" id="red-fort">
      <div class="flag-pos">
          <div class="flag">
              <img src="@/assets/flag-pole.png" alt="" class="pole" v-show="isStarted">
              <img src="@/assets/flag-h-2.gif" alt="" v-show="isStarted" class="banner" :class="{'hoisting': isPlaying}">
            </div>
      </div>
        <img src="@/assets/redfort.webp" alt="" class="red-port-img">
      </div>
      <modal name="my-first-modal" :adaptive="true" @opened="setFocus">
          <div class="modal-wrapper">
              <div class="content">
                <h5 class="title">Share</h5>
                <p>Enter name here</p>
                <div class="input-wrapper">
                  <input type="text" placeholder="Ex: John" v-model="share_name" id="share_name" autofocus>
                  <div class="info-txt" ref="info"></div>
                </div>
              </div>
              <div class="buttons">
                <div class="btn" @click.prevent="shareOn('link')">
                  <div class="icon">
                    <img src="@/assets/icons8-link-96.png" alt="">
                  </div>
                  <div class="text">Copy Link</div>
                </div>
                <div class="btn" @click.prevent="shareOn('whatsapp')">
                  <div class="icon">
                    <img src="@/assets/icons8-whatsapp-96.png" alt="">
                  </div>
                  <div class="text">WhatsApp</div>
                </div>
                <div class="btn" @click.prevent="shareOn('facebook')">
                  <div class="icon">
                    <img src="@/assets/icons8-facebook-96.png" alt="">
                  </div>
                  <div class="text">Facebook</div>
                </div>
              </div>
          </div>
      </modal>
  </div>
</template>

<script>
let music_player;
import {TweenMax, TweenLite, Sine, Linear} from "gsap/TweenMax";
export default {
  name: 'App',
  data(){
    return {
      isStarted: false,
      isPlaying: false,
      name: '',
      share_name: ''
    }
  },
  computed: {
    shareLink(){
      return  encodeURI(location.origin+'?name='+this.share_name);
    }
  },
  created(){
    this.initialize();
    this.name = this.getParameterByName('name');
  },
  methods: {
    share(){
      this.$modal.show('my-first-modal');
    },
    setFocus(){
      document.getElementById('share_name').focus();
    },
    copyText() {
      var dummy = document.createElement("textarea");
      document.body.appendChild(dummy);
      dummy.value = this.shareLink;
      if (navigator.userAgent.match(/ipad|ipod|iphone/i)) {
          dummy.contentEditable = true;
          dummy.readOnly = true;
          var range = document.createRange();
          range.selectNodeContents(dummy);
          var selection = window.getSelection();
          selection.removeAllRanges();
          selection.addRange(range);
          dummy.setSelectionRange(0, 999999);
      }else {
          dummy.select();
      }
      document.execCommand("copy");
      document.body.removeChild(dummy);
    },
    shareOn(loc){
      let text = this.shareLink;
      if(loc === 'link'){
        // copy link and share
        this.copyText();
        let info = this.$refs.info;
        info.innerHTML = 'Link Copied!';
        setTimeout(() => {
          info.innerHTML = '';
        }, 3000);

      }else if(loc === 'whatsapp'){
        // share on whatsapp
        text = text.replace(/%20/g, '%2520');
        window.open('https://wa.me/?text='+text, '_blank');
      }else if(loc === 'facebook'){
        // share on facebook
        text = text.replace(/%20/g, '%2520');
        window.open('https://www.facebook.com/sharer/sharer.php?u='+text, '_blank');
      }
    },
    initialize(){
      music_player = new Audio(require('@/assets/audio/republic-day.mp3'));
    },
    startMusic(){
      if(!this.isStarted){
          this.isStarted = true;
          console.log('Playing audio');
          setTimeout(() => {
            this.isPlaying = true;
            setTimeout(() => {
              this.startFlowerFall();
            }, 3000);
          }, 1000);
          if(typeof music_player !== 'undefined'){
            music_player.play();
          }
      }
    },
    startFlowerFall(){
      TweenLite.set("#app",{perspective:600})

        let total = 10;
        let container = document.getElementById("app"),	w = window.innerWidth , h = window.innerHeight;
        
        for (let i=0; i<total; i++){ 
          let Div = document.createElement('div');
          let Div2 = document.createElement('div');
          let Div3 = document.createElement('div');
          TweenLite.set(Div,{attr:{class:'dot'},x:R(0,w),y:R(-200,-150),z:R(-200,200),xPercent:"-50%",yPercent:"-50%"});
          TweenLite.set(Div2,{attr:{class:'dot2'},x:R(0,w),y:R(-200,-150),z:R(-200,200),xPercent:"-50%",yPercent:"-50%"});
          TweenLite.set(Div3,{attr:{class:'dot3'},x:R(0,w),y:R(-200,-150),z:R(-200,200),xPercent:"-50%",yPercent:"-50%"});
          container.appendChild(Div);
          container.appendChild(Div2);
          container.appendChild(Div3);
          animm(Div);
          animm2(Div2);
          animm3(Div3);
        }
        
        function animm(elm){
          TweenMax.to(elm,R(6,15),{y:h+100,ease:Linear.easeNone,repeat:-1,delay:-15});
          TweenMax.to(elm,R(4,8),{x:'+=100',rotationZ:R(0,180),repeat:-1,yoyo:true,ease:Sine.easeInOut});
          TweenMax.to(elm,R(2,8),{repeat:-1,yoyo:true,ease:Sine.easeInOut,delay:-5});
        }
          function animm2(elm){
          TweenMax.to(elm,R(6,15),{y:h+100,ease:Linear.easeNone,repeat:-1,delay:-25});
          TweenMax.to(elm,R(4,8),{x:'+=100',rotationZ:R(0,180),repeat:-1,yoyo:true,ease:Sine.easeInOut});
          TweenMax.to(elm,R(2,8),{repeat:-1,yoyo:true,ease:Sine.easeInOut,delay:-5});
        }
          function animm3(elm){
          TweenMax.to(elm,R(6,15),{y:h+100,ease:Linear.easeNone,repeat:-1,delay:-5});
          TweenMax.to(elm,R(4,8),{x:'+=100',rotationZ:R(0,180),repeat:-1,yoyo:true,ease:Sine.easeInOut});
          TweenMax.to(elm,R(2,8),{repeat:-1,yoyo:true,ease:Sine.easeInOut,delay:-5});
        }

        function R(min,max) {return min+Math.random()*(max-min)}
    },
    getParameterByName(name, url = window.location.href) {
        name = name.replace(/[\[\]]/g, '\\$&');
        let regex = new RegExp('[?&]' + name + '(=([^&#]*)|&|#|$)'),
            results = regex.exec(url);
        if (!results) return null;
        if (!results[2]) return '';
        return decodeURIComponent(results[2].replace(/\+/g, ' '));
    }
  },
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.modal-wrapper {
  font-size: 1rem;
  padding:1rem;

  .input-wrapper {
    position: relative;

    .info-txt {
      position: absolute;
      top: 71px;
    }
  }
  h5 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }
  p {
    font-size: 20px;
  }
  input {
    width: 100%;
    height: 45px;
    padding: 1rem;
    font-size: 20px;
    border: 1px solid #ddd;
    border-radius: 21px;
    margin-bottom: 2rem;
    margin-top: 16px;
  }

  .buttons {
    display: flex;
    align-items: center;
    justify-content: space-between;
    .btn {
      font-size: 18px;
      padding: 0.5rem 0.6rem;
      //border: 1px solid #ddd;
      border-radius: 4px;
      cursor: pointer;

      .icon {
        width: 70px;
        height: 70px;
        border-radius: 50%;
        margin: auto;
        margin-bottom: 10px;
        object-fit: cover;
        img {
          width: 100%;
          height: 100%;
           object-fit: cover;
        }
      }

      .text {
        font-size: 1rem;
      }
    }
  }
}
.bottom-fixed {
  position: fixed;
  bottom: 30px;
  z-index: 99;
  &.centered {
    left: 50%;
    transform: translateX(-50%);
  }
}

.click-here-btn {
  background-color: #000000ba;
  color: white;
  border-radius: 4px;
  font-weight: bold;
  border: 2px dashed white;
  cursor: pointer;
  padding: 0.5rem;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
  height: 100%;
  position: absolute;
  overflow: hidden;
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
  background: url('./assets/rose2.png');
  background-size: 100% 100%;
}
.dot2{
  width:35px;
  height:35px;
  position:absolute;
  background: url('./assets/rose.png');
  background-size: 100% 100%;
}
.dot3{
  width:35px;
  height:35px;
  position:absolute;
  background: url('./assets/rose3.png');
  background-size: 100% 100%;
}

.name {
  position: fixed;
  line-height: 1.5;
  top: 30px;
  width: 100%;
  left: 50%;
  transform: translateX(-50%);
  z-index: 99;
  font-size: 1.5erem;
  text-align: center;
  color: #fff;
  text-shadow: 0 0 0px #fff, 0 0 4px #fff, 0 0 7px #0073e6, 0 0 11px #0073e6, 0 0 5px #0073e6, 0 0 3px #0073e6, 0 0 5px #0073e6;
  animation: blinker 2s linear infinite;
}

@keyframes blinker {
  50% {
    opacity: 0;
  }
}
@keyframes glow {
  from {
    text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 15px #0073e6, 0 0 20px #0073e6, 0 0 25px #0073e6, 0 0 30px #0073e6, 0 0 35px #0073e6;
  }
  to {
    text-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #0073e6, 0 0 40px #0073e6, 0 0 50px #0073e6, 0 0 60px #0073e6, 0 0 70px #0073e6;
  }
}

</style>
