<template>
    <div class="list">
        <transition name='slide' mode="out-in" :enter-active-class="qa || qindex<3 ? 'animated fadeInRight' : 'animated delay zoomIn'" :leave-active-class="qa || qindex<3 ? 'animated fadeOutLeft' : 'animated zoomOut'">
            <img class="container" :src="bgimg" alt="shamie" :key='qa || qindex<3 ? "question"+qindex : "answer"' />
        </transition>
        <div class="clickarea" v-show="qa" @click='qa && next()'></div>
        <img class="logo" v-show="logoshow"  src="static/logo.png" />
        </div>
</template>
<script>
/* eslint-disable */

export default {
  data() {
    return {
      qindex: 0,
      aindex: 0,
      qa: true,
      logoshow: false,
      qlist: [
        "static/你好骗吗h5第1题.jpg",
        "static/你好骗吗h5第2题.jpg",
        "static/你好骗吗h5ad.jpg",
        "static/你好骗吗h5第3题.jpg"
      ],
      alist: [
        "static/result你好骗吗h5结果1.jpg",
        "static/result你好骗吗h5结果2.jpg",
        "static/result你好骗吗h5结果3.jpg",
        "static/result你好骗吗h5结果4.jpg",
        "static/result你好骗吗h5结果5.jpg"
      ],
      logo: ""
    };
  },
  computed: {
    bgimg: function() {
      if (this.qa || this.qindex < 3) {
        return this.qlist[this.qindex];
      } else {
        return this.alist[this.aindex];
      }
    }
  },
  methods: {
    next() {
      if (this.qindex < 3) {
        if (this.qindex == 1) {
          this.qa = false;
          setTimeout(() => {
            this.next();
            this.qa = true;
          }, 3000);
        }
        this.qindex++;
      } else {
        this.qa = false;
        this.aindex = Math.round(Math.random() * 4);
       let timecount=setInterval(() => {
         if((document.querySelector(".container").src).indexOf('result')!=-1){
          this.logoshow = true;
          let owidth = 640,
            oheight = 1136,
            swidth = window.screen.width,
            sheight = window.screen.height,
            c = document.createElement("canvas"),
            cxt = c.getContext("2d"),
            backingStoreRatio =
              cxt.webkitBackingStorePixelRatio ||
              cxt.mozBackingStorePixelRatio ||
              cxt.msBackingStorePixelRatio ||
              cxt.oBackingStorePixelRatio ||
              cxt.backingStorePixelRatio ||
              1,
            ratio = devicePixelRatio / backingStoreRatio;
          c.width = swidth * ratio;
          c.height = sheight * ratio;
          c.style.width = swidth + "px";
          c.style.height = sheight + "px";
          cxt.scale(ratio, ratio);

          const image1 = new Image();
          image1.crossOrigin = "Anonymous";
          image1.src = document.querySelector(".container").src;
          image1.onload = () => {
            cxt.drawImage(image1, 0, 0, swidth, sheight);

            const image2 = new Image();
            image2.crossOrigin = "Anonymous";

            image2.src = document.querySelector(".logo").src;
            image2.onload = () => {
              cxt.drawImage(
                image2,
                parseInt(swidth * 224 / owidth),
                parseInt(sheight * 762 / oheight),
                parseInt(swidth * 190 / owidth),
                parseInt(sheight * 190 / oheight)
              );
              let data = c.toDataURL(),
                image = `<img class='convert' src="${data}" />`;
              document.querySelector(".list").innerHTML = image;
            };
          };
           clearInterval(timecount);          
         }
        }, 100);
      }
      
    }
  }
};
</script>
<style>
div.list {
  position: relative;
}
div.clickarea {
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  height: 660px;
}
img.container,img.convert {
  width: 100%;
  height: 100%;
}
img.container{
  pointer-events: none;
}
img.logo {
  position: absolute;
  left: 224px;
  top: 762px;
  width: 190px;
  height: 190px;
  opacity:0;
  /* left: 3.484375rem; */
  /* bottom: 3.890625rem; */
  /* width: 2.96875rem; */
}
/*.slide-enter-active,.slide-leave-active{
        transition: all 1s;
    }
    .slide-enter,.slide-leave-to{
        opacity: 0;
        transform: translateX(30px);        
    }
    .slide-enter{
        transform: translateX(30px);        
    }
    .slide-leave-to{
        transform: translateX(-30px);        
    } */
</style>
