<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Lista de Clientes</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="primary" to="/customers/form">Adicionar</v-btn>
    </v-row>
    <p>Total de Clientes {{customers.length}}</p>
    <v-divider></v-divider>
    <div id="customers">
     <div v-for="(customer) in customers" :key="customer.id">
       <p>Name: {{customer.name}}</p>
       <p>Phone: {{customer.phone}}</p>
       <p>Email: {{customer.email}}</p>
       <p>cpf_cnpj: {{customer.cpf_cnpj}}</p>
        <v-btn depressed small color="primary" :to="`/customers/form?id=${customer.id}`">Editar</v-btn>
        <v-btn depressed small color="error" @click="matar(customer.id)">Delete</v-btn>
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
    customers: [],

    items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Clientes',
          disabled: true,
        },
      ],
   }
  },
  mounted () {
    return  axios.get('http://localhost:3000/api/v1/customers')
    .then(response => {
      var resp = response.data
      this.customers = resp.data
      console.log(this.customers)
    })
  },
  methods: {
    matar(id) {
      return axios.delete(`http://localhost:3000/api/v1/customers/${id}`)
      .then( response => { axios.get('http://localhost:3000/api/v1/customers')
        .then(response => {
          var resp = response.data
          this.updateList(resp.data)
        })
      })
    },
    updateList(newList){
    console.log("TEste updatedd")
      this.customers = newList
    }
  }
}
</script>
