<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Novo Cliente</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="warning" to="/customers/">VOLTAR</v-btn>
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
                v-model="customer.name"
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
                v-model="customer.phone"
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
                v-model="customer.email"
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
                v-model="customer.cpf_cnpj"
                :rules="cpf_cnpjRules"
                label="CPF / CNPJ"
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
      customer: {
        name: '',
        phone: '',
        cpf_cnpj: '',
        email: '',
      },
      edit: false,
      cpf_cnpjRules: [
        v => !!v || 'CPF/CNPJ é obrigatório',
        v => v.length <= 10 || 'CPF/CNPJ must be less than 10 characters',
      ],
      nameRules: [
        v => !!v || 'Nome é obrigatório',
        v => v.length <= 10 || 'Name must be less than 10 characters',
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],
      phoneRules: [
        v => !!v || 'E-mail is required',
      ],
      items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Clientes',
          disabled: false,
          exact: true,
          to: '/customers'
        },
        {
          text: 'Cadastrar cliente',
          disabled: true,
        },
      ],
    }),
    methods: {
      salvar() {
        if (this.edit == false){
          console.log("criar novo")
          console.log(this.customer)
          axios.post('http://localhost:3000/api/v1/customers/', {
            name: this.customer.name,
            phone: this.customer.phone,
            email: this.customer.email,
            cpf_cnpj: this.customer.cpf_cnpj,
          })
          .then(function (response) {
            console.log(this.customer);
          })
          .catch(function (error) {
            console.log(error);
          });
          this.$router.push('/customers')
        } else{
          console.log("editar else")
          axios.put(`http://localhost:3000/api/v1/customers/${this.$router.currentRoute.query.id}`, {
            name: this.customer.name,
            phone: this.customer.phone,
            email: this.customer.email,
            cpf_cnpj: this.customer.cpf_cnpj,
          })
          .then(function (response) {
            console.log(this.customer);
          })
          .catch(function (error) {
            console.log(error);
          });
          this.$router.push('/customers')
        }
      }
    },
    mounted() {
      console.log(this.$router.currentRoute.query.id)
      if (this.$router.currentRoute.query.id != null){
        this.edit = true;
        console.log(this.$router.currentRoute.query.id)
        console.log("if mounted")
        return  axios.get(`http://localhost:3000/api/v1/customers/${this.$router.currentRoute.query.id}`)
        .then(response => {
          var resp = response.data
          this.customer = resp.data
          console.log(this.customer)
        })
      }
    }
  }
</script>
