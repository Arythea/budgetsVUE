<template>
  <b-container>
    <b-row>
      <div cols="12">
        <h2>Pressupostos guardats</h2>
        <button @click="order('date','asc')">Data ascendent</button>
        <button @click="order('date','desc')">Data descendent</button>
        <button @click="order('project','asc')">Nom ascendent</button>
        <button @click="order('project','desc')">Nom descendent</button>
        <button @click="order('date','desc');searchString='';searchBudgets();">Reset</button>
        <input type="text" @keyup="searchBudgets()" v-model="searchString">
        <hr>
        <div v-for="(budget,index) in auxArrayBudgets" :key="index">
          <h3>{{ budget.project }}</h3>
          <small>{{ budget.date }} </small><br>
          <b>Client: </b>{{ budget.client }}
          <div v-for="(service,index) in budget.services" :key="index">
            <div v-if="service.seleccionat">
              {{ service.nom }}
              ({{ service.preu }} euros)
              <div v-if="service.mostraComponent">
                Extres:
                <ul>
                  <li>Pàgines: {{ budget.pages }}</li>
                  <li>Idiomes: {{ budget.languages }}</li>
                  <li><b>Preu extres:</b> {{ budget.extresWeb }}</li>
                </ul>
              </div>
            </div>
          </div>
          <b>Total: </b>{{ budget.total }} euros
          <br>
          Compartir: <br>
          <div class="url">
            <a :href="budget.sharingURL" target="_blank">{{ budget.sharingURL }}</a>
          </div>
          <hr>
        </div>
      </div>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: 'BudgetsList',
  data() {
    return {
      searchString: '',
      auxArrayBudgets: this.arrayBudgets
    }
  },
  methods: {
    order(field,direction) {
      let switchOrder = 1;
      if (direction == 'desc') {
        switchOrder = -1;
      }
      this.arrayBudgets.sort( (a, b) => {
        if(a[field] < b[field]) {
          return -1 * switchOrder;
        } else if (a[field] > b[field]) {
          return 1 * switchOrder;
        }
        return 0;
      } );
    },
    searchBudgets() {
      if(this.searchString == '') {
        this.auxArrayBudgets = this.arrayBudgets;
      } else {
        this.auxArrayBudgets = this.arrayBudgets.filter( (x) => x.project.includes(this.searchString));
      }
    }
  },
  props: ['arrayBudgets']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  background-color: #eee;
  width: 100%;  
}
.url {
  overflow-x: scroll;
}
</style>
