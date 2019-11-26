<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Lista de Demandas</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="primary" to="/demands/form">Adicionar</v-btn>
    </v-row>
    <p>Total de Demandas {{demands.length}}</p>

    <div id="demands">
     <div v-for="(demand) in demands" :key="demand.id">
       <p>Descrição: {{demand.description}}</p>
       <p>Observação: {{demand.observation}}</p>
       <p>Valor: {{demand.value}}</p>
       <p>Data de inicio: {{demand.date_start}}</p>
       <p>Data final: {{demand.date_end}}</p>
        <v-btn depressed small color="primary" :to="`/demands/form?id=${demand.id}`">Editar</v-btn>
        <v-btn depressed small color="error" @click="matar(demand.id)">Delete</v-btn>

       <v-divider></v-divider>
     </div>
    </div>
  </v-container>
</template>

<script>
import axios from 'axios'
export default {
  data() {
   return {
    demands: [],
    items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Demandas',
          disabled: true,
          href: '/demands/',
        }
      ],
   }
  },
  mounted () {
    return  axios.get('http://localhost:3000/api/v1/demands')
    .then(response => {
      var resp = response.data
      this.demands = resp.data
      console.log(this.demands)
    })
  },
  methods: {
    matar(id) {
      return axios.delete(`http://localhost:3000/api/v1/demands/${id}`)
      .then( response => { axios.get('http://localhost:3000/api/v1/demands')
        .then(response => {
          var resp = response.data
          this.updateList(resp.data)
        })
      })
    },
    updateList(newList){
    console.log("TEste updatedd")
      this.demands = newList
    }
  }
}
</script>
