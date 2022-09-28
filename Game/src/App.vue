<script>
import card from './components/card.vue'
import cardBottomBox from "./components/cardBottomBox.vue";
import threeCardBox from "./components/threeCardBox.vue";

export default {
  components: {
    card,
    cardBottomBox,
    threeCardBox
  },
  data() {
    return {
      cardList: new Map(),
      cardTypeNum:4,
      singleCardNum:1,
      bottomList: [],
      list: [],
      isGaming: false,
      isFailing: false,
      isSuccess: false,
      isSetting:true,
      threeCardList:[],
    }
  },

  methods: {
    judgeHide() {
      this.list = [];
      this.cardList.forEach((value) => {
        value.isHide = false;
        this.list.push(value);
      })
      for (let i = 0; i < this.list.length; i++) {
        let x = this.list[i].x;
        let y = this.list[i].y;//通过x,y可以推得位置
        for (let j = i + 1; j < this.list.length; j++) {//只有后面的卡片会盖住前面的
          if (this.list[j].x > x - 52 && this.list[j].x < x + 52 && this.list[j].y > y - 52 && this.list[j].y < y + 52) {
            this.list[i].isHide = true;
          }
        }
      }

    },
    remove(id) {
      if (this.cardList.get(id).isHide) {
        return;
      }
      this.listAdd([this.cardList.get(id).imgId,id]);
      this.cardList.delete(id);
      this.judgeHide();
      this.judgeClear();
      this.judgeResult();
    },
    judgeResult() {
      if (this.cardList.size === 0&&this.threeCardList.length===0) {
        this.showSuccess();
      }
      if (this.bottomList.length === 10) {
        this.showFail();
      }
    },
    listAdd(card) {
      console.log(card)
      for (let i = 0; i < this.bottomList.length; i++) {
        if (this.bottomList[i][0] === card[0]) {
          this.bottomList.splice(i, 0, card);
          return;
        }
      }
      this.bottomList.push(card)
    },
    judgeClear() {
      let numList = new Array(parseInt(this.cardTypeNum)).fill(0);
      console.log(numList)

      for (let i in this.bottomList) {
        numList[this.bottomList[i][0]]++;
        if (numList[this.bottomList[i][0]] === 3) {//如果某一个图片数量达到3，就消除
          this.bottomDelete(this.bottomList[i]);
        }
      }
    },
    bottomDelete(card) {
      this.bottomList = this.bottomList.filter((value) => {
        return value[0] !== card[0];
      })
    },
    showSuccess() {
      this.isSuccess = true;
      this.isGaming = false;
      this.isFailing = false;
    },
    showFail() {
      this.isGaming = false;
      this.isSuccess = false;
      this.isFailing = true;
    },
    freshGame() {
      this.isSuccess=false;
      this.isFailing=false;
      this.isSetting=false;
      this.isGaming=true;
      this.cardList.clear();
      this.bottomList=[];
      this.list=[];
      this.threeCardList=[];
      class Card {
        constructor(isHide, imgId, id, x, y) {
          this.isHide = isHide;//是否被覆盖
          this.id = id;//卡片唯一id
          this.imgId = imgId;//图片id
          this.x = x;//x偏移量
          this.y = y;//y偏移量
        }
      }
      for (let i = 0; i < this.cardTypeNum * 3 * this.singleCardNum; i++) {
        this.cardList.set(i, new Card(false, i % this.cardTypeNum, i, this.setOffsetX(), this.setOffsetY()))//初始化所有图片,后面的会盖住前面的
      }
      this.judgeHide()
    },
    freshCard() {
      this.cardList.forEach((card)=>{
        card.x=this.setOffsetX();
        card.y=this.setOffsetY();
      })
      this.judgeHide();

    },
    removeThree() {
      for(let i = 0;i<3;i++) {
        if(this.bottomList[0]) {
          this.threeCardList.push(this.bottomList.shift());
        }
      }
    },
    removeFromThree(card) {
      this.threeCardList=this.threeCardList.filter((value)=>{
        return value[1]!==card[1]
      })


      this.listAdd(card);
      this.judgeHide();
      this.judgeClear();
      this.judgeResult();

    },
    setOffsetX() {//设置偏移量
      return 26 * Math.floor(16 * Math.random())//x偏移n格（最多8格)
    },
    setOffsetY() {
      return 26 * Math.floor(16 * Math.random())//y偏移n格（最多8格)
    },
    changeDefault() {
      this.isSetting=true;
      this.isGaming=false;
      this.isFailing=false;
      this.isSuccess=false;
    }


  },
  created() {

  }
}
</script>

<template>
  <template v-if="isGaming">
    <div class="card_container">
      <card v-for="card in list" :x="card.x" :y="card.y" :is-hide="card.isHide" :img-id="card.imgId" @removeFromContainer="remove(card.id)"></card>
    </div>
    <three-card-box :card-list="threeCardList" @removeFromThreeContainer="removeFromThree($event)"></three-card-box>
    <card-bottom-box :card-list="bottomList" class="bottom_container" ></card-bottom-box>
    <div style="position: absolute;left: 800px">
      <button @click="removeThree">取三张</button>
      <button @click="freshCard">洗牌</button>
      <button @click="freshGame">重新开始</button>
    </div>

  </template>
  <template v-if="isSuccess">
    <h3>你赢了！</h3>
    <button @click="freshGame">再来一局</button>
    <button @click="changeDefault">修改配置</button>
  </template>
  <template v-if="isFailing">
    <h3>你输了！</h3>
    <button @click="freshGame">再来一局</button>
    <button @click="changeDefault">更改配置</button>
  </template>
  <template v-if="isSetting">
    卡片种类：<input type="range" min="4" max="10" step="1" v-model="cardTypeNum">{{cardTypeNum}}<br>
    卡片数量：<input type="range" min="1" max="5" v-model="singleCardNum">{{singleCardNum}} * {{cardTypeNum}} * 3<br>
    <button @click="freshGame">开始游戏！</button>
  </template>
</template>

<style scoped>
  .bottom_container {
    top: 500px;
  }
</style>
