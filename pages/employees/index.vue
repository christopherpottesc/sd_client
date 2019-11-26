<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Lista de Funcionários</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="primary" to="/employees/form">Adicionar</v-btn>
    </v-row>
    <p>Total de Funcionários {{employees.length}}</p>
    <v-divider></v-divider>

    <div id="employees">
     <div v-for="(employee) in employees" :key="employee.id">
       <p>Name: {{employee.name}}</p>
       <p>Phone: {{employee.phone}}</p>
       <p>Email: {{employee.email}}</p>
       <p>Função: {{employee.function}}</p>
       <v-btn depressed small color="primary" :to="`/employees/form?id=${employee.id}`">Editar</v-btn>
       <v-btn depressed small color="error" @click="matar(employee.id)">Delete</v-btn>
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
    employees: [],
    items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Funcionários',
          disabled: true,
        }
      ],
   }
  },
  mounted () {
    return  axios.get('http://localhost:3000/api/v1/employees')
    .then(response => {
      var resp = response.data
      this.employees = resp.data
      console.log(this.employees)
    })
  },
  methods: {
    matar(id) {
      return axios.delete(`http://localhost:3000/api/v1/employees/${id}`)
      .then( response => { axios.get('http://localhost:3000/api/v1/employees')
        .then(response => {
          var resp = response.data
          this.updateList(resp.data)
        })
      })
    },
    updateList(newList){
    console.log("TEste updatedd")
      this.employees = newList
    }
  }
}
</script>
