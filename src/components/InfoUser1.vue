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
  </div>
</template>

<script>
export default {
  props: ['talkData'],
  data: () => ({ checked: false }),
  computed: {
    users () {
      const data = new Map()
      this.talkData.users.forEach(u => {
        if (data.has(u)) data.set(u, data.get(u) + 1)
        else data.set(u, 1)
      })
      return {
        original: [...data],
        sorted: [...data].sort((a, b) => b[1] - a[1])
      }
    }
  }
}
</script>
