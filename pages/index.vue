<template>
  <v-container>

    <v-row no-gutters class="d-flex align-center">
      <h1>Dashboard Administrativa</h1>
    </v-row>

    <v-row class="mt-8" no-gutters >
        <v-col class="pr-2" cols="4" xl="4" lg="4">
          <v-card outlined >
            <v-list-item three-line>
              <v-list-item-content>
                <v-list-item-title class="headline mb-1">Empregados</v-list-item-title>
                <div class="overline mb-4">Quantidade de empregados:</div>
                <v-list-item-subtitle>{{ dashboard.employees }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-card-actions>
              <v-btn to="/employees/">Saiba mais</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

        <v-col class="pr-2" cols="4" xl="4" lg="4">
          <v-card  outlined >
            <v-list-item three-line>
              <v-list-item-content>
                <v-list-item-title class="headline mb-1">Clientes</v-list-item-title>
                <div class="overline mb-4">Quantidade de clientes:</div>
                <v-list-item-subtitle>{{ dashboard.customers }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-card-actions>
              <v-btn to="/customers/">Saiba mais</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

        <v-col cols="4" xl="4" lg="4">
          <v-card  outlined >
            <v-list-item three-line>
              <v-list-item-content>
                <v-list-item-title class="headline mb-1">Demandas</v-list-item-title>
                <div class="overline mb-4">Quantidade de demandas:</div>
                <v-list-item-subtitle>{{ dashboard.calls }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-card-actions>
              <v-btn to="/demands/">Saiba mais</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

        <v-col class="mt-8 pr-2" cols="4" xl="4" lg="4">
          <v-card  outlined >
            <v-list-item three-line>
              <v-list-item-content>
                <v-list-item-title class="headline mb-1">OS</v-list-item-title>
                <div class="overline mb-4">Quantidade de ordens de serviços:</div>
                <v-list-item-subtitle> {{ dashboard.calls }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-card-actions>
              <v-btn to="/calls/">Saiba mais</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

        <v-spacer/>

        <v-col class="mt-8 pr-2" cols="4" xl="4" lg="4">
          <v-card  outlined >
            <v-list-item three-line>
              <v-list-item-content>
                <v-list-item-title class="headline mb-1">Financeiro</v-list-item-title>
                <div class="overline mb-4">Finanças de vistorias:</div>
                <v-list-item-subtitle>
                  <span>Pagamento: R$ {{dashboard.total_value_calls}}</span>
                  |
                  <span>Deslocamento: R$ {{dashboard.total_value_displacement}}</span>
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-card-actions>
              <v-btn to="/finances/">Saiba mais</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

        <div class="teste1" :style="{backgroundColor: '#fff', marginTop: '20px'}">
        <p style="color: #000">Quantidade de vistorias por cidade:</p>
          <component
            :is="chartSelect.chartType"
            :chartdata="teste2"
          >
          </component>
        </div>
    </v-row>
  </v-container>
</template>

<script>
import AppBarChart from "@/components/charts/BarChart.vue";
import axios from 'axios'

export default {
  components: {
    AppBarChart
  },
  data() {
   return {
    dashboard: [],
    chartSelect: {
      selected: "bar",
      chartType: "AppBarChart"
    },
        teste2: {
          labels: [],
          datasets: [{
              label: "",
              backgroundColor: "black",
              borderColor: "black",
              fill: false,
              lineTension: 0,
              data: ["1", "2", "5"]
          }]
        }
   }
  },
  methods: {
    updateChart(){
      this.teste2.labels = Object.keys(this.dashboard.citys_values)
      this.teste2.datasets[0].data = Object.values(this.dashboard.citys_values)

    }
  },
  mounted () {
    return  axios.get('http://localhost:3000/api/v1/dashboard')
    .then(response => {
      var resp = response.data
      this.dashboard = resp.data
      console.log(this.dashboard)
      this.updateChart()
    })
  }
}
</script>

