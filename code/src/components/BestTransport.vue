<template>
  <div class="title">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand class="ml-2">
        <b>{{ appName }}</b>
      </b-navbar-brand>
    </b-navbar>

    <div class="center-all">
      <div class="shipping-container">
        <InputData :cities="cities" @formResponse="((e) => formResponseData = e)" ref="formComponentRef" />
        <PriceResult :fastest="getFastest()" :cheapest="getCheapest()" />
        <b-button v-if="formResponseData" variant="primary" class="button-absolute" @click="resetForm" >Limpar</b-button>
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

import axios from 'axios'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
    InputData,
    PriceResult
  },
  data() {
    const appName = ''
    let transportData = []
    let cities = []
    let formResponseData = null;

    return {
      appName,
      transportData,
      cities,
      formResponseData,
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina
    axios.get('http://api.localhost:3000/transport')
      .then(response => {
        this.transportData = response.data;
        this.cities = this.getCities(response.data);
      })
      .catch(error => {
        console.error(error);
      });

    this.appName = 'Melhor Frete'
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    resetForm() {
      this.$refs.formComponentRef.resetForm();
      this.formResponseData = null;
    },

    getCities(arrayOfTransport) {
      let cities_arr = [];
      arrayOfTransport.forEach(elem => {
        if (!cities_arr.includes(elem.city)) {
          cities_arr.push(elem.city);
        }
      })
      return cities_arr
    },

    getCheapest() {
      try {
        if (!this.formResponseData) return null;
        const destination = this.formResponseData.destination
        const weight = this.formResponseData.weight
        let res = {};
        let cheapest = Infinity;
        if (weight > 100) {
          this.transportData.forEach(elem => {
            const cost_heavy = parseFloat(elem.cost_transport_heavy.replace(/[^\d.]/g, ''));
            if (cost_heavy < cheapest
              && elem.city === destination) {
              cheapest = cost_heavy;
              res = elem;
            }
          })
        } else {
          this.transportData.forEach(elem => {
            const cost_light = parseFloat(elem.cost_transport_light.replace(/[^\d.]/g, ''));
            if (cost_light < cheapest
              && elem.city === destination) {
              cheapest = cost_light;
              res = elem;
            }
          })
        }
        return {
          name: res.name,
          cost: weight * cheapest,
          lead_time: res.lead_time
        }
      } catch (err) {
        alert('Erro: ' + err)
        return null
      }

    },

    getFastest() {
      try {
        if (!this.formResponseData) return null;
        const destination = this.formResponseData.destination;
        const weight = this.formResponseData.weight;
        const weightHeavy = weight > 100;
        let res = {};
        let price;
        let fastest = 'init';

        this.transportData.forEach(elem => {
          if (this.compareHours(elem.lead_time, fastest)
            && elem.city === destination) {
            fastest = elem.lead_time;
            price = (weightHeavy ? elem.cost_transport_heavy : elem.cost_transport_light)
            res = elem;
          }
        })

        return {
          name: res.name,
          cost: weight * parseFloat(price.replace(/[^\d.]/g, '')),
          lead_time: res.lead_time
        }
      } catch (err) {
        alert('Erro: ' + err)
        return null
      }
    },

    compareHours(h1, h2) {
      if (h2 === 'init') return h1;
      let [h1format, h2format] = [h1, h2]
      if (h1[h1.length] === 'h') {
        h1format = h1format.slice(0, -1);
        h1format + '00';
      } else {
        h1format = h1format.slice(0, -3) + h1format.slice(-2, h1format.length);
      }
      if (h2[h2.length] === 'h') {
        h2format = h2format.slice(0, -1);
        h2format + '00';
      } else {
        h2format = h2format.slice(0, -3) + h2format.slice(-2, h2format.length);
      }

      if (parseInt(h1format) < parseInt(h2format)) return h1;
      return h2;
    }
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
  width: 80vw;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  border-radius: 20px;
  padding: 1.3rem;
  position: relative;
}

.center-all {
  display: flex;
  justify-content: center;
  margin-top: 4rem;
}

.button-absolute {
    position: absolute;
    bottom: 1.3rem;
    right: 1.3rem;
  }

@media (max-width: 600px) {
  .shipping-container {
    width: 100vw;
  }
}
</style>
