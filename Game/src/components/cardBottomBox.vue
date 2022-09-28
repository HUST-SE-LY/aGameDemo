<template>
  <div class="container">
    <card v-for="(card,index) in cardList" :img-id="card[0]" :card-id="card[1]" :in-bottom="true"></card>
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