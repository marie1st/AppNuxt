<template>
    <div class="container text-center">
    <div>
        <h1 class="title">Covid-19 World Graph</h1>
    </div>
     <div class="links">
        <button @click="$fetch">Yesterday</button>
        <button @click="$fetch2">2 Days Ago</button>
        <p>{{dateTime}}</p>
    </div>
    <div><p><bar-chart :data="barChartData" :options="barChartOptions" :height="200" /></p></div>
    <div><p>Why Chart is not appear</p></div>
    </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import 'ant-design-vue/dist/antd.css'
export default {
  extends: Bar,
  props: ['data', 'options'],
  watch: {
    test: {
      immediate: false
    }
  },
  mounted () {
    this.renderChart(this.data, this.options)
  },
  name: 'chart',
  data () {
    return {
      covids: [],
      dateTime: Date.now(),
      barChartData: {
        labels: [
          'casess',
          'todaycases',
          'deaths',
          'todayDeaths',
          'recovered',
          'todayRecovered',
          'active',
          'critical',
          'casesPerOneMillion',
          'deathsPerOneMillion',
          'tests',
          'testsPerOneMillion',
          'population',
          'oneCasePerpeople',
          'oneDeathPerPeople',
          'oneTestPerPeople',
          'activePerOneMillion',
          'recoverPerOneMillion',
          'criticalPerOneMillion',
          'affectedCountries'
        ],
        datasets: [
          {
            label: 'Yesterday',
            data: [10, 20, 30, 24, 21, 23, 14, 10, 15, 18, 19, 48, 28, 28, 28, 18, 18, 17, 17, 18],
            backgroundColor: '#003f5c'
          },
          {
            label: '2 days ago',
            data: [10, 20, 30, 24, 21, 23, 14, 10, 15, 18, 19, 48, 28, 28, 28, 18, 18, 17, 17, 18],
            backgroundColor: '#2f4b7c'
          }
        ]
      },
      barChartOptions: {
        responsive: true,
        legend: {
          display: false
        },
        title: {
          display: true,
          text: 'Covid Worldwide Analytic',
          fontSize: 24,
          fontColor: '#6b7280'
        },
        tooltips: {
          backgroundColor: '#17BF62'
        },
        scales: {
          xAxes: [
            {
              gridLines: {
                display: false
              }
            }
          ],
          yAxes: [
            {
              ticks: {
                beginAtZero: true
              },
              gridLines: {
                display: false
              }
            }
          ]
        }
      }
    }
  },
  async fetch () {
    this.covids = await this.$http.$get('https://disease.sh/v3/covid-19/all?yesterday=true&twoDaysAgo=false&allowNull=false').then(res => res.json())
  },

  async fetch2 () {
    this.covids = await this.$http.$get('https://disease.sh/v3/covid-19/all?yesterday=false&twoDaysAgo=true&allowNull=false').then(res => res.json())
  },
  // call fetch only on client-side
  fetchOnServer: false
}
</script>

<style>
.container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content:center;
    align-items: center;
    text-align: center;
}

</style>
