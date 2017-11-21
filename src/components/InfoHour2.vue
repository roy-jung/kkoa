<template>
  <div class="wrap">
    <select @change="selectUser">
      <option selected disabled hidden value=""/>
      <option v-for="(user, i) in users" :key="(user + i)" :value="user">{{user}}</option>
    </select>
    <label><input type="checkbox" v-model="checked"/>정렬</label>

    <div class="row">
      <div class="table">
        <table>
          <colgroup>
            <col width="70%"/>
            <col/>
          </colgroup>
          <tr v-for="(date, i) in (checked ? userData.sorted : userData.original)" :key="date[0]">
            <td><strong>{{date[0]}}</strong></td>
            <td>{{date[1]}}</td>
          </tr>
        </table>
      </div>

      <div class="chart">
        <ChartLine
          :chart-data="this.userData.chartData"
          :options="{
            responsive: true,
            maintainAspectRatio: true
          }"
        />
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import ChartLine from './ChartLine'

export default {
  components: { ChartLine },
  props: ['talkData'],
  data: () => ({
    checked: false,
    selectedUser: null
  }),
  computed: {
    users () {
      return [...this.talkData.users.reduce((p, c) => p.add(c), new Set())].sort((a, b) => a.localeCompare(b))
    },
    userData () {
      if (!this.selectedUser) {
        return {
          original: [],
          sorted: [],
          chartData: {
            labels: [],
            datasets: [{ label: '', data: [] }]
          }
        }
      }
      const data = new Map()
      this.talkData.users.forEach((u, i) => {
        if (u === this.selectedUser) {
          const dd = moment(this.talkData.dates[i]).format('YY.MM.DD HH') + ':00 ~'
          if (data.has(dd)) data.set(dd, data.get(dd) + 1)
          else data.set(dd, 1)
        }
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
          label: `${this.selectedUser}의 시간대별 대화량`,
          backgroundColor: '#f87979',
          data: []
        }]
      })
      return res
    }
  },
  methods: {
    selectUser (e) {
      this.selectedUser = e.target.value
    }
  }
}
</script>

<style scoped>
.row {
  margin-top: 20px;
  height: 100%;
}
.row:after {
  content: '';
  display: block;
  clear: both;
}
.table {
  width: 40%;
  float: left;
  height: 100%;
  overflow-y: auto;
}

.chart {
  width: 60%;
  float: right;
}
</style>
