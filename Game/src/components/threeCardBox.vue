<template>
  <div class="container">
    <card v-for="card in cardList" :img-id="card[0]" :key="card[1]" :x="card[2]*54" :y="0" :card-id="card[1]" :is-hide="false" @removeFromContainer="$emit('removeFromThreeContainer',card)"></card>
  </div>
</template>

<script>
  import card from "./card.vue";
  export default {
    components:{card},
    props:['cardList'],
    emits:['removeFromThreeContainer']

  }
</script>

<style scoped>
  .container {
    position: absolute;
    display: flex;
    top: 500px;
    left: calc(200px + 10 * 54px + 54px);

  }
</style>