<template>

</template>

<script lang="ts">
const complement = function (value:any) {
  return value < 10 ? `0${value}` : value
}

export default (date:any) => {
  const time = new Date(date)
  const year = time.getFullYear()
  const month = complement(time.getMonth() + 1)
  const day = complement(time.getDate())
  const hour = complement(time.getHours())
  const minute = complement(time.getMinutes())
  const second = complement(time.getSeconds())
  const week = '星期' + '日一二三四五六'.charAt(time.getDay())
  return `${hour}:${minute}:${second} \n  ${year}-${month}-${day} ${week} `
}
</script>

<style scoped>

</style>