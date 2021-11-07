<script>
import { ref, computed } from 'vue'

export default {
  name: 'Slider',
  props: {
    modelValue: {
      type: [Number, String, null],
      required: true
    },
    min: {
      type: Number,
      default: 0
    },
    max: {
      type: Number,
      default: 100
    }
  },

  setup (props, { emit }) {
    const value = computed(() => typeof props.modelValue === 'string' ? parseFloat(props.modelValue) : props.modelValue)
    const bgWidth = computed(() => 100 * (Math.min(props.max, Math.max(props.min, value.value)) - props.min) / (props.max - props.min))
    const slider = ref(null)

    const updateState = (ev) => {
      const pageX = ev.pageX
      const rect = slider.value.getBoundingClientRect()
      const x = pageX - rect.left
      const width = rect.right - rect.left
      const value = Math.min(props.max, Math.max(props.min, props.min + x / width * (props.max - props.min)))
      emit('update:modelValue', value)
    }

    /**
     * @param {PointerEvent} ev
     */
    const onMouseDown = (ev) => {
      updateState(ev)
      ev.preventDefault()
      window.addEventListener('mousemove', onMouseMove)
      window.addEventListener('mouseup', onMouseUp)
    }

    const onMouseMove = ev => updateState(ev)

    const onMouseUp = () => {
      window.removeEventListener('mousemove', onMouseMove)
      window.removeEventListener('mouseup', onMouseUp)
    }

    return {
      bgWidth,
      onMouseDown,
      value,
      slider
    }
  }
}
</script>

<template>
  <div
    ref="slider"
    class="slider"
    style="width: 100%;"
    :style="{'background-size': `${bgWidth}% 100%`}"
    @mousedown="onMouseDown"
  />
</template>

<style scoped lang="scss">
@import "../base.scss";

.slider {
  height: 25px;
  display: block;
  position: relative;
  cursor: ew-resize;
  border: 1px solid $background-color;
  background-color: $input-background-color;
  background-image: linear-gradient(90deg, $number-color, $number-color);
  background-size: 0% 100%;
  background-repeat: no-repeat;
}
</style>
