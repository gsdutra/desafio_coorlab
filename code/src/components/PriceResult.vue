<template>
  <div class="price-result">
    <h5 v-if="!cheapest">
      Nenhum dado selecionado
    </h5>
    <h5 v-if="cheapest">
      Estas são as melhores alternativas de frete que encontramos para você:
    </h5>

    <TransportInfo
      v-if="cheapest"
      text="Frete com menor valor"
      :data="cheapest"
      color1="#e0e0df"
      color2="#bdd3d5"
      icon_path=""
    />
    <TransportInfo
      v-if="fastest"
      text="Frete mais rápido"
      :data="fastest"
      color1="#efeff2"
      color2="#95b4c7"
      icon_path=""
    />

    <b-button v-if="cheapest" variant="primary" class="button-absolute">Limpar</b-button>
  </div>
</template>

<script>
import TransportInfo from './TransportInfo.vue';
export default {
  props: {
    cheapest: Object,
    fastest: Object
  },
  components: {
    TransportInfo
  }
}
</script>

<style>
  .price-result {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    height: 40rem;
    padding: 3rem;
    width: 60%;
    position: relative;
  }

  .button-absolute {
    position: absolute;
    bottom: 0;
    right: 0;
  }
</style>