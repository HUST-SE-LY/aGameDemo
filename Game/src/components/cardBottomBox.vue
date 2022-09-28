<template>
  <div class="container">
    <card v-for="(cardId,index) in cardList" :img-id="cardId" :in-bottom="true"></card>
  </div>
</template>

<script>
  import card from './card.vue'
  export default {
    components:{
      card,
    },
    data() {
      return {
        cardList:this.cardList,
      }
    },
    props:['cardList']
  }
</script>

<style scoped>
  .container {
    width: calc(54px * 10);
    height: 54px;
    border: grey 1px solid;
    position: absolute;
    top: 300px;
    left: 200px;
    display: flex;
  }
</style>