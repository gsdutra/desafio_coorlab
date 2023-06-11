<template>
  <div class="flex">
    <div class="box"
    :style="{'background-color':color1}"
    style="width: 21vw;">
    <div class="imgdiv" :style="{'background-color':color2}">
      <img v-if="iconHand" src="../assets/hand-coin.svg"/>
      <img v-if="iconClock" src="../assets/clock-check.svg"/>
    </div>
      <div class="text-align">
        <a style="font-weight: bold;">{{ text }}</a>
        <a>Transportadora: {{ data.name }}</a>
        <a>Tempo estimado: {{ data.lead_time }}</a>
      </div>
    </div>
    <div class="box"
    :style="{'background-color':color1}"
    style="width: 12vw; margin-left: .7rem;">
      <div class="text-align">
        <a style="font-weight: bold;">Preço:</a>
        <a>{{ data.cost.toLocaleString('pt-br',{style: 'currency', currency: 'BRL'}) }}</a>
      </div>
    </div>
  </div>
  </template>
  
<script>
  export default {
    props: {
      text: String,
      data: Object,
      color1: String,
      color2: String,
      iconHand: Boolean,
      iconClock: Boolean
    }
}
</script>

<style>
  .box {
    min-height: 6rem;
    margin-top: 1rem;
    border-radius: 20px;
    display: flex;
    overflow: hidden;
  }
  .text-align {
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: .6rem;
  }
  .imgdiv {
    display: flex;
    justify-content: center;
    align-items: center;
    padding-left: 6%;
    width: 100px;
  }
  .flex {
    display: flex;
  }
</style>