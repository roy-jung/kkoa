<template>
  <div>
    <label><input type="checkbox" v-model="checked"/>정렬</label>
    <table>
      <colgroup>
        <col width="50"/>
        <col/>
        <col/>
      </colgroup>
      <tr v-for="(user, i) in (checked ? users.sorted : users.original)" :key="user[0]">
        <th>{{i + 1}}</th>
        <td><strong>{{user[0]}}</strong></td>
        <td>{{user[1]}}</td>
      </tr>
    </table>

    <div class="chart">
      <ChartDoughnut
        :data="this.users.chartData"
        :options="{
          responsive: true,
          maintainAspectRatio: false
        }"
        :height="800"
      />
    </div>
  </div>
</template>

<script>
import ChartDoughnut from './ChartDoughnut'

const colors = [
  '#f99494',
  '#f8cc8c',
  '#a1dbb1',
  '#d5dadc',
  '#f66364',
  '#f5b04d',
  '#71c989',
  '#bac1c4',
  '#f33334',
  '#f29b1d',
  '#4cba6b',
  '#9ea8ad',
  '#dc0d0e',
  '#de890d',
  '#3fa45b',
  '#848f94',
  '#b90c0d',
  '#c67a0c',
  '#358a4d',
  '#69767c',
  '#930a0a',
  '#a4650a',
  '#2a6d3c',
  '#596468'
]

export default {
  components: {ChartDoughnut},
  props: ['talkData'],
  data: () => ({ checked: false }),
  computed: {
    users () {
      const data = new Map()
      this.talkData.users.forEach(u => {
        if (data.has(u)) data.set(u, data.get(u) + 1)
        else data.set(u, 1)
      })
      const res = {
        original: [...data],
        sorted: [...data].sort((a, b) => b[1] - a[1])
      }
      res.chartData = res.sorted.reduce((p, c, i) => {
        p.labels.push(c[0])
        p.datasets[0].data.push(c[1])
        p.datasets[0].backgroundColor.push(colors[i % 24])
        return p
      }, {
        labels: [],
        datasets: [{
          label: '유저별 대화량',
          data: [],
          backgroundColor: []
        }]
      })
      return res
    }
  }
}
</script>
