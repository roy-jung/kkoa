<template>
  <div id="app">
    <InputBox @setData="setData"/>
    <Menus @setDescType="setDescType" :show="!!talkData"/>
    <p :class="talkData ? '' : 'hidden'">
      {{talkData && talkData.dates[0]}} ~ {{talkData && talkData.dates[talkData.dates.length - 1]}}
    </p>
    <component :is="descType" :talkData="talkData"/>
  </div>
</template>

<script>
import InputBox from './components/InputBox'
import Menus from './components/Menus'
import InfoHour1 from './components/InfoHour1'
import InfoHour2 from './components/InfoHour2'
import InfoUser1 from './components/InfoUser1'
import InfoUser2 from './components/InfoUser2'

export default {
  components: {
    InputBox,
    Menus,
    InfoHour1,
    InfoUser1,
    InfoHour2,
    InfoUser2
  },
  data: () => ({
    talkData: null,
    descType: null
  }),
  methods: {
    setData (d) {
      this.talkData = d
    },
    setDescType (v) {
      this.descType = v
    }
  }
}
</script>

<style>
html, body, #app, .wrap {
  height: 100%;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
}
th, td {
  border: 1px solid #999;
}

.hidden {
  display: none;
}

.chart {
  width: 100%;
}
</style>
