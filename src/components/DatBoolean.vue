<script>
export default {
  name: 'DatBoolean',
  props: {
    modelValue: {
      type: Boolean,
      default: false
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
  <li class="control-item boolean">
    <label ref="label">
      <span class="label-text">{{label}}</span>
      <div class="control">
        <input type="checkbox" ref="checkbox" :checked="modelValue" @change="x => $emit('update:modelValue', x.target.checked)" />
      </div>
    </label>
  </li>
</template>
