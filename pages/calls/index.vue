<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Lista de Ordens de Serviços</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="primary" to="/calls/form">Adicionar</v-btn>
    </v-row>
    <p>Total de Ordens de serviços {{calls.length}}</p>
    <v-divider></v-divider>

    <div id="demands">
     <div v-for="(call) in calls" :key="call.id">
       <p>Atividade: {{call.activity}}</p>
       <p>Nome do contato: {{call.name}}</p>
       <p>Telefone: {{call.phone}}</p>
       <p>Valor: {{call.value}}</p>
       <p>Valor de deslocamento: {{call.value_displacement}}</p>
       <p>Prazo: {{call.deadline}}</p>
       <p>Data de abertura: {{call.start_date}}</p>
       <p>CEP: {{call.cep}}</p>
       <p>Cidade: {{call.city}}</p>
       <p>Bairro: {{call.neighborhood}}</p>
       <p>Rua: {{call.street}}</p>
       <p>Número: {{call.number}}</p>
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
    calls: [],
    items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Ordens de serviços',
          disabled: true,
        },
      ],
   }
  },
  mounted () {
      // axios
      //   .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      //   .then(response => (this.info = response))
      //   console.log("Oiiiiiii caralho")

    return  axios.get('http://localhost:3000/api/v1/calls')
    .then(response => {
      var resp = response.data
      this.calls = resp.data
      console.log(this.calls)
    })
  }
}
</script>
