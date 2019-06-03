<template>
  <div :class="[{'empty-tile': number === null}, {'green-tile': placed}]"
        :style="`width: calc(${tileSize} - ${marginSize});height: calc(${tileSize} - ${marginSize});margin-right: ${marginSize};margin-bottom: ${marginSize}; font-size:${parseInt(tileSize)/5}vw`"
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
    tileSize: {
      type: String,
      required: false,
      default: null,
    },    
    marginSize: {
      type: String,
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