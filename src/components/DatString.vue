<script>
export default {
  props: {
    modelValue: {
      type: String,
      default: ''
    },
    label: {
      type: String,
      default: ''
    }
  },
  emits: ['update:modelValue']
}
</script>

<template>
  <li class="control-item string">
    <label ref="label">
      <span class="label-text">{{label}}</span>
      <div class="control">
        <input type="text" :value="modelValue" @input="ev => $emit('update:modelValue', ev.target.value)"/>
      </div>
    </label>
  </li>
</template>

<style scoped lang="scss">
.control-item.string {
  .control {
    display: inline-flex;
  }
}
</style>
