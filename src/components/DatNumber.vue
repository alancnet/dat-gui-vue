<script>
import Slider from './Slider.vue'
import { computed } from 'vue'

export default {
  name: 'DatNumber',
  components: { Slider },
  props: {
    modelValue: {
      type: [String, Number, null],
      required: true
    },
    min: {
      type: Number,
      default: Number.NEGATIVE_INFINITY
    },
    max: {
      type: Number,
      default: Number.POSITIVE_INFINITY
    },
    step: {
      type: Number,
      default: undefined
    },
    label: {
      type: String,
      default: ''
    },
    showSlider: {
      type: Boolean,
      default: true
    }
  },
  setup (props, { emit }) {
    const minValue = computed(() => Math.min(props.min, props.max))
    const maxValue = computed(() => Math.max(props.min, props.max))
    const hasSlider = computed(() => Number.isFinite(minValue.value) && Number.isFinite(maxValue.value) && props.showSlider)
    const stepValue = computed(() => {
      if (!props.step) {
        const val = maxValue.value - minValue.value
        return (10 ** Math.floor(Math.log(Math.abs(val)) / Math.LN10)) / 10
      } else {
        return props.step
      }
    })
    const sanitizeNumber = number => {
      if (number === '') {
        emit('update:modelValue', null)
      } else {
        const [min, max, step] = [minValue.value, maxValue.value, stepValue.value]
        let safeNumber = Math.min(max, Math.max(min, number))
        if (step && Number.isFinite(step)) {
          safeNumber = Math.round(safeNumber / step) * step
        }
        emit('update:modelValue', parseFloat(safeNumber.toPrecision(15)))
      }
    }

    return {
      minValue,
      maxValue,
      stepValue,
      hasSlider,
      sanitizeNumber
    }
  }
}
</script>

<template>
  <li class="control-item number">
    <label ref="label">
      <span class="label-text">{{label}}</span>
      <div class="control">
        <Slider
          v-show="hasSlider"
          class="slider"
          :min="minValue"
          :max="maxValue"
          :model-value="modelValue"
          @update:model-value="sanitizeNumber">
        </Slider>
        <input
          type="number"
          ref="input"
          class="input"
          :min="minValue"
          :max="maxValue"
          :step="stepValue"
          :value="modelValue === null ? '' : modelValue"
          @input="x => sanitizeNumber(x.target.value)"
        />
      </div>
    </label>
  </li>
</template>

<style scoped lang="scss">
.control-item.number {
  .control {
    display: inline-flex;

    .slider {
      flex: 3;
    }
    .input {
      flex: 1;
    }
  }

  input[type="number"] {
    -moz-appearance: textfield;
  }
  input[type="number"]::-webkit-inner-spin-button,
  input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
}
</style>
