<script>
  export default {
    data() {
      return {
        imgList:['src/assets/img1.jpg','src/assets/img2.jpg','src/assets/img3.jpg','src/assets/img4.jpg','src/assets/img5.png','src/assets/img6.png','src/assets/img7.png','src/assets/img8.png','src/assets/img9.png','src/assets/img10.png'],
        isHide:this.isHide,
        cardId:this.cardId,
        isInBottom:this.inBottom,
        type:this.type,
      }
  },
    props:['imgId','cardId','inBottom','type','x','y','isHide'],
    emits:['removeFromContainer'],




  }
</script>

<template>
<div :class="isInBottom?'container_relative':'card_container'" :style="`top:${y}px;left:${x}px`" @click="$emit('removeFromContainer',this.imgId)">
  <img :src="imgList[this.imgId]" alt="" class="card_img">
  <div class="hide_box" v-if="isHide"></div>
</div>
</template>

<style scoped>
  .card_img {
    width: 50px;
    height: 50px;
    display: block;
  }
  .card_container {
    position: absolute;
    padding: 2px;
    background-color: darkmagenta;
    cursor: pointer;
  }
  .hide_box {
    position: absolute;
    top: 2px;
    left: 2px;
    background-color: rgba(10,10,10,0.7);
    width: 50px;
    height: 50px;
  }
  .container_relative {
    position: relative;
    padding: 2px;
    background-color: darkmagenta;
  }
</style>
