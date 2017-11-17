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
        <td><strong>{{date[0]}}:00~</strong></td>
        <td>{{date[1]}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  props: ['talkData'],
  data: () => ({ checked: false }),
  computed: {
    dates () {
      const data = new Map()
      this.talkData.dates.forEach(d => {
        const dd = moment(d).format('YY.MM.DD HH')
        if (data.has(dd)) data.set(dd, data.get(dd) + 1)
        else data.set(dd, 1)
      })
      return {
        original: [...data],
        sorted: [...data].sort((a, b) => b[1] - a[1])
      }
    }
  }
}
</script>
