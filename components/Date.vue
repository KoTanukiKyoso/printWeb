<template>
  <div style="text-align: end;" :class="size">
    <span>{{ date.split("-")[0] }}年 </span>
    <span>{{ date.split("-")[1] }}月 </span>
    <span>{{ date.split("-")[2] }}日 </span>
  </div>
</template>

<script>
export default {
  props: {
    date: {
      type: String,
    },
    size: {
      type: String,
      default: "min"
    },
  },
}
</script>

<style scoped>

</style>
