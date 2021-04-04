<template>
  <div style="text-align: start;" :class="size">
    <span>{{ text }} </span>
  </div>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
    },
    size: {
      type: String,
      default: "normal"
    },
  },
}
</script>

<style scoped>

</style>
