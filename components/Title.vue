<template>
  <div style="text-align: center; margin-top: 5mm;" :class="size">
    <span v-for="c of title">{{ c }} </span>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
    },
    size: {
      type: String,
      default: "x-large"
    }
  },
}
</script>

<style scoped>

</style>
