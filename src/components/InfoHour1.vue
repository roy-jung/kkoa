<template>
  <div>
    <label><input type="checkbox" v-model="checked"/>정렬</label>
    <table>
      <colgroup>
        <col width="50"/>
        <col/>
        <col/>
      </colgroup>
      <tr v-for="(date, i) in (checked ? dates.sorted : dates.original)" :key="date[0]">
        <th>{{i + 1}}</th>
        <td><strong>{{date[0]}}</strong></td>
        <td>{{date[1]}}</td>
      </tr>
    </table>

    <div class="chart">
      <ChartLine
        :chart-data="this.dates.chartData"
        :options="{
          responsive: true,
          maintainAspectRatio: true
        }"
      />
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import ChartLine from './ChartLine'

export default {
  components: { ChartLine },
  props: ['talkData'],
  data () {
    return { checked: false }
  },
  computed: {
    dates () {
      const data = new Map()
      this.talkData.dates.forEach(d => {
        const dd = moment(d).format('YY.MM.DD HH') + ':00 ~'
        if (data.has(dd)) data.set(dd, data.get(dd) + 1)
        else data.set(dd, 1)
      })
      const res = {
        original: [...data],
        sorted: [...data].sort((a, b) => b[1] - a[1])
      }
      res.chartData = res.original.reduce((p, c) => {
        p.labels.push(c[0])
        p.datasets[0].data.push(c[1])
        return p
      }, {
        labels: [],
        datasets: [{
          label: '시간대별 대화량',
          backgroundColor: '#f87979',
          data: []
        }]
      })
      return res
    }
  }
}
</script>