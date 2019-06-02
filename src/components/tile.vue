<template>
  <div :class="[{'empty-tile': number === null}, {'green-tile': placed}]"
        class="tile">{{ number }}</div>
</template>
<script>
export default {
  props: {
    number: {
      type: Number,
      required: false,
      default: null,
    },
    placed: {
      type: Boolean,
      required: true,
      default: false,
    }
  }
}
</script>