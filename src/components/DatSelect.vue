<script>
import { computed } from 'vue'

export default {
  name: 'DatSelect',
  props: {
    modelValue: {
      type: [String, Number, null],
      required: true
    },
    /**
     * Array of {name, value}, or array of values
     */
    items: {
      type: Array,
      required: true
    },
    label: {
      type: String,
      default: ''
    }
  },
  setup (props) {
    /** @typedef {{name: String, value: String|Number}} Item */
    /** @type {import('vue').ComputedRef<Item[]>} */
    const myItems = computed(() => props.items.map(x => typeof x === 'object' ? x : { name: (x || '').toString(), value: x }))
    return {
      myItems
    }
  }
}
</script>

<template>
  <li class="control-item select">
    <label ref="label">
      <span class="label-text">{{label}}</span>
      <div class="control">
        <select :value="modelValue" @change="ev => $emit('update:modelValue', ev.target.value)">
          <option
            v-for="item in myItems"
            :key="item.value"
            :value="item.value">
            {{item.name}}
          </option>
        </select>
      </div>
    </label>
  </li>
</template>

<style scoped>
  select { width: 100%; }
</style>
