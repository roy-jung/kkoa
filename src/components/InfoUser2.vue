<template>
  <div>
    <label><input type="checkbox" v-model="checked"/>정렬</label>
    <table>
      <colgroup>
        <col width="50"/>
        <col width="30%"/>
        <col/>
      </colgroup>
      <tr v-for="(date, i) in (checked ? userData.sorted : userData.original)" :key="date[0]">
        <th>{{i + 1}}</th>
        <td><strong>{{date[0]}}:00~ ({{date[1].size}})</strong></td>
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
    userData () {
      const data = new Map()
      this.talkData.dates.forEach((d, i) => {
        const dd = moment(d).format('YY.MM.DD HH')
        if (data.has(dd)) data.set(dd, data.get(dd).add(this.talkData.users[i]))
        else {
          const set = new Set()
          set.add(this.talkData.users[i])
          data.set(dd, set)
        }
      })
      return {
        original: [...data],
        sorted: [...data].sort((a, b) => b[1].size - a[1].size)
      }
    }
  }
}
</script>
