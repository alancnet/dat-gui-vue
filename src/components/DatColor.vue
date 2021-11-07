<script>
import { computed, ref } from 'vue'
import { ColorPicker } from 'vue-color-kit'

export default {
  name: 'DatColor',
  components: { ColorPicker },
  props: {
    modelValue: {
      type: String,
      required: true
    },
    label: {
      type: String,
      default: ''
    }
  },
  emits: ['update:modelValue'],
  setup (props, { emit }) {
    const showColorPicker = ref(false)
    const textColor = computed(() => {
      if (props.modelValue[0] !== '#') { return 'black' }
      const r = parseInt(props.modelValue.substr(1, 2) || '0', 16)
      const g = parseInt(props.modelValue.substr(3, 2) || '0', 16)
      const b = parseInt(props.modelValue.substr(5, 2) || '0', 16)
      const yiq = ((r * 299) + (g * 587) + (b * 114)) / 1000
      return (yiq >= 128) ? 'black' : 'white'
    })

    const onChangeColor = (ev) => {
      console.log('onChangeColor', ev)
      emit(
        'update:modelValue',
        ev.rgba.a < 1
          ? ev.hex + Math.floor(ev.rgba.a * 256).toString(16).padStart(2, 0).toUpperCase()
          : ev.hex
      )
    }

    return {
      showColorPicker,
      textColor,
      onChangeColor
    }
  }
}
</script>

<template>
  <li
    class="control-item color"
    :style="{ 'border-left-color': modelValue }"
    @mouseleave="showColorPicker = false">
    <label>
      <span class="label-text">{{label}}</span>
      <div class="control" @mouseover="showColorPicker = true">
        <input
          type="text"
          :value="modelValue"
          :style="{ 'background-color': modelValue, 'color': textColor }"
          readonly/>
        <ColorPicker
          v-show="showColorPicker"
          :color="modelValue"
          class="color-picker"
          @changeColor="onChangeColor"
          theme="dark"
        />
      </div>
    </label>
  </li>
</template>

<style scoped lang="scss">
@import "../base.scss";
@import '~vue-color-kit/dist/vue-color-kit.css';
.control-item.color {
  input[type="text"] {
    text-align: center;
  }

  .control > div {
    position: absolute;
    right: 0;
    width: 200px;
    z-index: 1;
  }
}
</style>
