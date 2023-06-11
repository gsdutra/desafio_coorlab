<template>
  <div class="input-data">
    <h5>
      <img src="../assets/map.png"> 
      <a>Insira o destino e o peso</a>
    </h5>

    <b-form @submit="onSubmit" v-if="show" ref="formRef" class="justify-form">
      <b-form-group id="input-group-1" label="Destino" label-for="city">
        <b-form-select
          id="city"
          v-model="form.destination"
          :options="[ { text: 'Selecione o destino', value: null }, ...destinations ]"
        ></b-form-select>
      </b-form-group>

      <b-form-group id="input-group-2" label="Peso" label-for="weight">
        <b-form-input
          id="weight"
          type="number"
          v-model="form.weight"
          placeholder="Peso da carga em kg"
        ></b-form-input>
      </b-form-group>

      <button style="align-self: center;" type="submit" class="mt-3 default-button">Analisar</button>
    </b-form>

    <ModalWarning :showModal="showModal" :message="modalMessage"
    @close="()=>showModal=false"/>
  </div>
</template>

<script>
import ModalWarning from './ModalWarning.vue'
export default {
  components: {
    ModalWarning
  },
  props: {
    cities: Array
  },
  data() {
    return {
      form: {
        weight: null
      },
      show: true,
      showModal: false,
      modalMessage: ''
    }
  },
  computed: {
    destinations() {
      return this.cities;
    }
  },
  mounted() {
    setTimeout(() => {
      this.$refs.formRef.reset();
    }, 0);
  },
  methods: {
    onSubmit(event) {
      event.preventDefault()
      if (!this.form.weight || !this.form.destination) {
        this.modalMessage = 'Insira os valores para realizar a análise.';
        this.showModal = true;
      } else {
        this.$emit('formResponse', this.form)
      }
    },
    resetForm() {
      this.$refs.formRef.reset();
    }
  },
}
</script>

<style>
.input-data {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #DEDEDE;
  border-radius: 20px;
  height: 70vh;
  padding: 1rem;
  width: 30vw;
}

img {
  width: 37px;
  height: auto;
  margin-right: 1rem;
}

.justify-form {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  padding: 1rem;
  width: 30vw;
}

.default-button {
  background-color: #93b4c3;
  color: black;
  border: none;
  padding-left: 3rem;
  padding-right: 3rem;
  max-width: 14rem; 
  padding-top: .5rem;
  padding-bottom: .5rem;
  border-radius: 8px;
}

@media (max-width: 600px) {
  .input-data {
    width: 80vw;
    height: 45vh;
  }
  .justify-form {
    margin-top: 0;
    width: 70vw;
  }
}
</style>
