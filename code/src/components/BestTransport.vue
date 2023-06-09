<template>
  <div class="title">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand class="ml-2">
        <b>{{ appName }}</b>
      </b-navbar-brand>
    </b-navbar>
    
    <div class="center-all">
      <div class="shipping-container">
          <InputData/>
          <PriceResult/>
      </div>
    </div>
  </div>

</template>

<script>
import {
  BNavbar,
  BNavbarBrand,
} from 'bootstrap-vue'

import InputData from './InputData.vue'
import PriceResult from './PriceResult.vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
    InputData,
    PriceResult
  },
  data() {
    const appName = ''

    return {
      appName,
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina
    this.appName = 'Melhor Frete'
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    methodFoo() {
      console.log(this.appName)
    },
  },
}
</script>

<style scoped>
.title .navbar {
  background-color: #00aca6 !important;
}

.title .navbar-brand {
  margin-left: 20px;
}

.shipping-container {
  display: flex;
  justify-content: center;
  width: 70%;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  border-radius: 20px;
  padding: 20px;
}

.center-all {
  display: flex;
  justify-content: center;
  margin-top: 4rem;
}
</style>
