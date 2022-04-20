<template>
  <b-container class="home mt-5">
    <b-row>
      <b-col cols="9">
        <form action="">
          <p>¿Qué quieres hacer?</p>
          <input type="text" placeholder="Project Name" v-model="projectName">
          <input type="text" placeholder="Client Name" v-model="clientName">
          <div v-for="(service,index) in services" :key="index">
            <input type="checkbox" @change="changeService(index)" :checked="service.seleccionat">
            {{ service.nom }} ({{ service.preu }}€)
            <div v-if="service.seleccionat && service.mostraComponent">
              <PanellWeb @getExtresWeb="setExtresWeb" :childPages="pages" :childLanguages="languages" />
            </div>
          </div>
          <p>Preu: {{ total }}</p>
        </form>
        <b-button class="bg-success" @click="saveBudget">SAVE</b-button>

        <b-button @click="$router.push('/')">BACK</b-button>
        <button @click="checkValues">Check</button>
      </b-col>
      <b-col cols="3">
        <BudgetsList :array-budgets="arrayBudgets" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
// @ is an alias to /src
import PanellWeb from '@/components/PanellWeb.vue'
import BudgetsList from '@/components/BudgetsList.vue'

export default {
  name: 'HomeView',
  components: {
    PanellWeb,
    BudgetsList
  },
  data() {
    return {
      projectName: "",
      clientName: "",
      services: [
        {
          nom: "Una pàgina web",
          alias: "web",
          preu: "500",
          seleccionat: false,
          mostraComponent: true
        },
        {
          nom: "Una consultoria SEO",
          alias: "seo",
          preu: "300",
          seleccionat: false,
          mostraComponent: false
        },
        {
          nom: "Una campanya de Google Ads  ",
          alias: "sem",
          preu: "200",
          seleccionat: false,
          mostraComponent: false
        }
      ],
      pages: 1,
      languages: 1,
      extresWeb: 0,
      arrayBudgets: [],
      baseURL: window.location.origin + this.$route.path
    }
  },
  computed: {
    total() {
      let auxTotal = this.services.reduce( (x,y) => {
        if(y.seleccionat) {
          return { preu: parseFloat(x.preu) + parseFloat(y.preu) };
        } else {
          return { preu: parseFloat(x.preu) };
        }
      }, {preu: 0});
      if(this.services[0].seleccionat) auxTotal.preu += this.extresWeb;
      return auxTotal.preu;
    }
  },
  methods: {
    changeService(index) {
      this.services[index].seleccionat = !this.services[index].seleccionat;
    },
    setExtresWeb(val, pages, languages) {
      this.extresWeb = val;
      this.pages = pages;
      this.languages = languages;
    },
    saveBudget() {
      let dt = new Date();
      let dateyyyyMMddHHiiss = `${dt.getFullYear().toString().padStart(4, '0')}-${(dt.getMonth()+1).toString().padStart(2, '0')}-${dt.getDate().toString().padStart(2, '0')} ${dt.getHours().toString().padStart(2, '0')}:${dt.getMinutes().toString().padStart(2, '0')}:${dt.getSeconds().toString().padStart(2, '0')}`;
      let budget = {
        client: this.clientName,
        project: this.projectName,
        services: this.services.map( service => { return {...service} } ),
        pages: this.pages,
        languages: this.languages,
        extresWeb: this.extresWeb,
        total: this.total,
        date: dateyyyyMMddHHiiss,
        sharingURL: this.getSharingURL()
      }
      this.arrayBudgets.push(budget);
      this.refresh();
    },
    getSharingURL() {
      let sharingURL = `${this.baseURL}?projectName=${this.projectName}&clientName=${this.clientName}`;
      for (let service of this.services){
        sharingURL += `&${service.alias}=${service.seleccionat}`;
      }
      sharingURL += `&pages=${this.pages}&languages=${this.languages}&extresWeb=${this.extresWeb}`;
      return sharingURL;
    },
    refresh() {
      this.projectName = '';
      this.clientName = '';
      this.services = [
        {
          nom: "Una pàgina web",
          preu: "500",
          seleccionat: false,
          mostraComponent: true
        },
        {
          nom: "Una consultoria SEO",
          preu: "300",
          seleccionat: false,
          mostraComponent: false
        },
        {
          nom: "Una campanya de Google Ads  ",
          preu: "200",
          seleccionat: false,
          mostraComponent: false
        }
      ],
      this.pages = 0,
      this.languages = 0,
      this.extresWeb = 0
    },
    checkValues() {
      console.log("pages: ",this.pages);
      console.log("languages: ",this.languages);
    }
  },
  created() {
  },
  mounted() {
    this.projectName = this.$route.query.projectName ? this.$route.query.projectName : "";
    this.clientName = this.$route.query.clientName ? this.$route.query.clientName : "";
    this.extresWeb = this.$route.query.extresWeb ? this.$route.query.extresWeb : 0;
    this.pages = this.$route.query.pages ? parseInt(this.$route.query.pages) : 1;
    this.languages = this.$route.query.languages ? parseInt(this.$route.query.languages) : 1;
    for(let service of this.services) {
      if(this.$route.query[service.alias]!== undefined) {
        service.seleccionat = false;
        if(this.$route.query[service.alias] == 'true') {
          service.seleccionat = true;
        }
      }
    }
  }
}
</script>