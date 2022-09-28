<template>
  <div class="container">
    <card v-for="card in cardList" :img-id="card[0]" :card-id="card[1]" :is-hide="false" :in-bottom="true" @removeFromContainer="$emit('removeFromThreeContainer',card)"></card>
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
    left: 800px;
  }
</style>