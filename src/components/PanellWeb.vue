<template>
  <b-container>
    <b-row>
      <div class="col-md-3">
        <div class="card p-3">
          <label for="num_paginas">Número de páginas</label>
          <div class="d-flex">
            <b-button @click="numPaginas--" class="d-inline-block w-25 bg-danger">-</b-button>
            <input class="d-inline-block w-50 text-center" type="number" v-model.number="numPaginas" name="num_paginas" min="0">
            <b-button @click="numPaginas++" class="d-inline-block w-25 bg-success">+</b-button> <b-icon-info-circle-fill v-b-modal.modal-1 />
          </div>
          <label for="num_idiomas">Número de idiomas</label>
          <div class="d-flex">
            <b-button @click="numIdiomas--" class="d-inline-block w-25 bg-danger">-</b-button>
            <input class="d-inline-block w-50 text-center" type="number" v-model.number="numIdiomas" name="num_idiomas" min="0">
            <b-button @click="numIdiomas++" class="d-inline-block w-25 bg-success">+</b-button> <b-icon-info-circle-fill v-b-modal.modal-2 />
          </div>
        </div>
        <p> {{ total }} </p>
      </div>
    </b-row>
    <div>

      <b-modal id="modal-1" title="Pages Field" hide-footer>
        <p class="my-4">This field will set the number of pages you want in your website</p>
      </b-modal>

      <b-modal id="modal-2" title="Languages Field" hide-footer>
        <p class="my-4">This field will set the number of languages you want in your website</p>
      </b-modal>
    </div>
  </b-container>
</template>

<script>
export default {
  name: 'PanellWeb',
  props: ['childPages','childLanguages'],
  data() {
    return {
      numPaginas: 1,
      numIdiomas: 1
    }
  },
  computed: {
    total() {
      let total = this.numPaginas * this.numIdiomas * 30;
      console.log("child-total pages: ", this.numPaginas);
      this.$emit('getExtresWeb', total, this.numPaginas, this.numIdiomas);
      return total;
    }
  },
  watch: {
    numPaginas(newVal) {
      if(newVal < 1) this.numPaginas = 1;
    },
    numIdiomas(newVal) {
      if(newVal < 1) this.numIdiomas = 1;
    }
  },
  created() {
    this.numPaginas = this.childPages;
    this.numIdiomas = this.childLanguages;
    console.log("child pages: ", this.numPaginas);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
