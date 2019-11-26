<template>
  <v-container>
    <v-breadcrumbs class="px-0" :items="items"></v-breadcrumbs>

    <v-row no-gutters class="d-flex align-center">
      <h1>Nova Ordem de Serviço</h1>
      <v-spacer></v-spacer>
      <v-btn depressed small color="warning" to="/calls/">VOLTAR</v-btn>
    </v-row>
    <v-divider></v-divider>
    <div id="customers">
      <p>Anexe o arquivo:</p>

        <v-container>
          <vue-dropzone ref="myVueDropzone" id="dropzone" :options="dropzoneOptions"></vue-dropzone>
        </v-container>
      <v-row>
      <v-spacer></v-spacer>
      <v-btn depressed color="success" to="/calls/">SALVAR</v-btn>
      </v-row>
    </div>
  </v-container>
</template>

<script>
import vue2Dropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'

  export default {
    components: {
      vueDropzone: vue2Dropzone
    },
    data(){
      return {
        dropzoneOptions: {
          url: 'http://localhost:3000/api/v1/imports',
          thumbnailWidth: 150,
          maxFilesize: 100,
          headers: { "My-Awesome-Header": "header value" },
          dictDefaultMessage: 'Arraste ou clice aqui para adicionar um arquivo .txt',
          uploadMultiple: false,
        },
        items: [
        {
          text: 'Dashboard',
          disabled: false,
          to: '/',
        },
        {
          text: 'Ordens de serviços',
          disabled: false,
          exact: true,
          to: '/calls'
        },
        {
          text: 'Nova ordem de serviço',
          disabled: true,
        },
      ],
      }
    },
  }
</script>
