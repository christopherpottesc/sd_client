<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Nova Demanda</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="warning" to="/demands/">VOLTAR</v-btn>
    </v-row>
    <v-divider></v-divider>
    <div id="customers">
      <p>Preencha o formulário:</p>

      <v-form v-model="valid">
        <v-container>
          <v-row>
            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="demand.description"
                :rules="descriptionRules"
                :counter="10"
                label="Descrição"
                required
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="demand.observation"
                :rules="descriptionRules"
                :counter="10"
                label="Observação"
                required
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="demand.value"
                :rules="valueRules"
                label="Valor"
                required
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
              md="3"
            >
              <v-text-field
                v-model="demand.date_start"
                :rules="dateRules"
                label="Data de inicio"
                required
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
              md="3"
            >
              <v-text-field
                v-model="demand.date_end"
                :rules="dateRules"
                label="Data de termino"
                required
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      <v-btn depressed color="success" @click="salvar">SALVAR</v-btn>

      </v-form>
      <v-row>
      <v-spacer></v-spacer>
      </v-row>
    </div>
  </v-container>
</template>

<script>
import axios from 'axios'

  export default {
    data: () => ({
      valid: false,
      demand: {
        description: '',
        observation: '',
        value: '',
        date_start: '',
        date_end: ''
      },
      edit: false,
      descriptionRules: [
        v => !!v || 'Nome é obrigatório',
        v => v.length <= 10 || 'Name must be less than 10 characters',
      ],
      dateRules: [],
      valueRules: [],
      items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Demandas',
          disabled: false,
          exact: true,
          to: '/demands'
        },
        {
          text: 'Cadastrar demanda',
          disabled: true,
        },
      ],
    }),
    methods: {
      salvar() {
        if (this.edit == false){
          console.log("salvar")
          console.log(this.demand)
          axios.post('http://localhost:3000/api/v1/demands/', {
            description: this.demand.description,
            observation: this.demand.observation,
            value: this.demand.value,
            date_start: this.demand.date_start,
            date_end: this.demand.date_end,
          })
          .then(function (response) {
            console.log(this.demand);
          })
          .catch(function (error) {
            console.log(error);
          });
          this.$router.push('/demands')
        } else{
          console.log("editar else")
          axios.put(`http://localhost:3000/api/v1/demands/${this.$router.currentRoute.query.id}`, {
            description: this.demand.description,
            observation: this.demand.observation,
            value: this.demand.value,
            date_start: this.demand.date_start,
            date_end: this.demand.date_end,
          })
          .then(function (response) {
            console.log(this.demand);
          })
          .catch(function (error) {
            console.log(error);
          });
          this.$router.push('/demands')
        }
      }
    },
    mounted() {
      console.log(this.$router.currentRoute.query.id)
      if (this.$router.currentRoute.query.id != null){
        this.edit = true;
        console.log(this.$router.currentRoute.query.id)
        console.log("if mounted")
        return  axios.get(`http://localhost:3000/api/v1/demands/${this.$router.currentRoute.query.id}`)
        .then(response => {
          var resp = response.data
          this.demand = resp.data
          console.log(this.demand)
        })
      }
    }
  }
</script>
