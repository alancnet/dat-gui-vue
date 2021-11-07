<script>
export default {
  name: 'DatButton',
  props: {
    label: {
      type: String,
      default: ''
    }
  },
  emits: ['click']
}
</script>
<template>
  <li class="control-item button">
    <label ref="label" @click="ev => $emit('click', ev)">
      {{label}}
    </label>
  </li>
</template>
<style scoped>
  label { width: 100% }
</style>
