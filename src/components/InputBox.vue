<template>
  <div>
    <h1>카카오톡 대화분석</h1>
    <input type="file" @change="readFile">
  </div>
</template>

<script>
const parse = r => {
  const lines = r.split('\n')
  const res = {dates: [], users: [], msgs: []}
  lines.shift()
  lines.forEach((v, i) => {
    const d = v.split(',"')
    if (d.length === 3) {
      res.dates.push(d[0])
      res.users.push(d[1].slice(0, -1))
      res.msgs.push(d[2].slice(0, -1))
    }
  })
  return res
}

export default {
  methods: {
    readFile (e) {
      const f = e.target.files[0]
      if (!f) return

      const r = new FileReader()
      r.onload = e => {
        const d = parse(e.target.result)
        this.$emit('setData', d)
        r.onload = null
      }
      r.readAsText(f)
    }
  }
}
</script>
