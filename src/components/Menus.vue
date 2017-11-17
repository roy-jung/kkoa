<template>
  <ul :class="!show ? 'hidden' : ''">
    <li v-for="menu in menus" :key="menu.type" :class="menu.type === selected ? 'selected' : ''">
      <button @click="setDescType(menu.type)">{{menu.text}}</button>
    </li>
  </ul>
</template>

<script>
export default {
  props: ['show'],
  data: () => ({
    selected: null,
    menus: [
      {type: 'InfoHour1', text: '시간대별 대화량'},
      {type: 'InfoUser1', text: '유저별 대화량'},
      {type: 'InfoUser2', text: '시간대별 활동유저'}
    ]
  }),
  methods: {
    setDescType (t) {
      this.selected = t
      this.$emit('setDescType', t)
    }
  }
}
</script>

<style scoped>
  ul {
    list-style: none;
    padding: 0;
  }
  li {
    display: inline-block;
  }
  button {
    margin: 0 2px;
    line-height: 1.5;
    padding: 5px 10px;
    background-color: #fff;
    border: 1px solid #07c;
    border-radius: 0;
  }
  li.selected button {
    background-color: #cdf;
  }
</style>
