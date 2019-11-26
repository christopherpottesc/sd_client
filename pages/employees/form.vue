<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Novo Empregado</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="warning" to="/employees/">VOLTAR</v-btn>
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
                v-model="employee.name"
                :rules="nameRules"
                :counter="10"
                label="Nome completo"
                required
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="employee.phone"
                :rules="phoneRules"
                :counter="10"
                label="Telefone"
                required
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="employee.email"
                :rules="emailRules"
                label="E-mail"
                required
              ></v-text-field>
            </v-col>

            <v-col
              cols="12"
              md="6"
            >
              <v-text-field
                v-model="employee.function"
                label="Função"
                required
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
        <v-btn depressed color="success" @click="salvar()">SALVAR</v-btn>
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
      employee: {
        name: '',
        phone: '',
        function: '',
        email: '',
      },
      edit: false,
      phoneRules: [
        v => !!v || 'Nome é obrigatório',
        v => v.length <= 10 || 'Name must be less than 10 characters',
      ],
      nameRules: [
        v => !!v || 'Nome é obrigatório',
        v => v.length <= 10 || 'Name must be less than 10 characters',
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],
      items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Funcionários',
          disabled: false,
          exact: true,
          to: '/employees'
        },
        {
          text: 'Cadastrar funcionário',
          disabled: true,
        },
      ],
    }),
   methods: {
      salvar() {
        if (this.edit == false){
          console.log("salvar")
          console.log(this.employee)
          axios.post('http://localhost:3000/api/v1/employees/', {
            name: this.employee.name,
            phone: this.employee.phone,
            function: this.employee.function,
            email: this.employee.email,
          })
          .then(function (response) {
            console.log(this.employee);
          })
          .catch(function (error) {
            console.log(error);
          });
          this.$router.push('/employees')
        }else{
          console.log("editar else")
          axios.put(`http://localhost:3000/api/v1/employees/${this.$router.currentRoute.query.id}`, {
            name: this.employee.name,
            phone: this.employee.phone,
            function: this.employee.function,
            email: this.employee.email,
          })
          .then(function (response) {
            console.log(this.employee);
          })
          .catch(function (error) {
            console.log(error);
          });
          this.$router.push('/employees')
        }
      }
    }
  }
</script>
