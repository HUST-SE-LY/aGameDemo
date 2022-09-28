<script>
import card from './components/card.vue'
import cardBottomBox from "./components/cardBottomBox.vue";

export default {
  components:{
    card,
    cardBottomBox
  },
  data() {
    return {
      cardList:[],
    }
  },

  methods:{
    judgeHide() {
      for(let i = 0 ;i<this.cardList.length;i++) {
        let x=this.cardList[i].x;
        let y=this.cardList[i].y;//通过x,y可以推得位置
        for(let j = i+1;j<this.cardList.length;j++) {
          if(this.cardList[j].x>x-52&&this.cardList[j].x<x+52&&this.cardList[j].y>y-52&&this.cardList[j].y<y+52) {
            this.cardList[i].isHide=true;
          }
        }
      }
    }
  },
  created() {//生成棋盘
    class Card {
      constructor(isHide,imgId,id,x,y) {
        this.isHide=isHide;//是否被覆盖
        this.id=id;//卡片唯一id
        this.imgId=imgId;//图片id
        this.x=x;//x偏移量
        this.y=y;//y偏移量
      }
    }
    function setOffsetX() {//设置偏移量
      return 26*Math.floor(16*Math.random())//x偏移n格（最多8格)
    }
    function setOffsetY() {
      return 26*Math.floor(16*Math.random())//y偏移n格（最多8格)
    }
    for(let i = 0;i<4*3*15;i++) {
      this.cardList.push(new Card(false,i%4,i,setOffsetX(),setOffsetY()))//初始化所有图片,后面的会盖住前面的
    }
    this.judgeHide()

  }
}
</script>

<template>
  <div class="card_container">
    <card v-for="card in cardList" :x="card.x" :y="card.y" :is-hide="card.isHide" :img-id="card.imgId" ></card>
  </div>
  <card-bottom-box :card-list="[0,1,2]" class="bottom_container"></card-bottom-box>
</template>

<style scoped>
  .bottom_container {
    top: 500px;
  }
</style>
