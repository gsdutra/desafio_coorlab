<template>
  <div class="input-data">
    <h5>
      <img src="../assets/map.png"> 
      <a>Insira o destino e o peso</a>
    </h5>

    <b-form @submit="onSubmit" v-if="show" ref="formRef">
      <b-form-group id="input-group-1" label="Destino" label-for="input-1">
        <b-form-select
          id="input-1"
          v-model="form.destination"
          :options="[ { text: 'Selecione o destino', value: null }, ...destinations ]"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group id="input-group-2" label="Peso" label-for="input-2">
        <b-form-input
          id="input-2"
          type="number"
          v-model="form.weight"
          placeholder="Peso da carga em kg"
          required
        ></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary" class="mt-3">Analisar</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  props: {
    cities: Array
  },
  data() {
    return {
      form: {
        weight: null
      },
      show: true
    }
  },
  computed: {
    destinations() {
      return this.cities;
    }
  },
  methods: {
    onSubmit(event) {
      event.preventDefault()
      this.$emit('formResponse', this.form)
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
  height: 40rem;
  width: 40vw;
  padding: 1rem;
}

img {
  width: 37px;
  height: auto;
  margin-right: 1rem;
}
</style>
