<script>
import { ref, computed } from 'vue'
export default {
  name: 'DatFolder',
  props: {
    label: {
      type: String,
      default: ''
    },
    open: {
      type: Boolean,
      default: undefined
    }
  },
  emits: ['update:open'],
  setup (props, { emit }) {
    const localOpen = ref(true)
    const onClick = () => {
      localOpen.value = !localOpen.value
      emit('update:open', !props.open)
    }
    const isOpen = computed(() => props.open ?? localOpen.value)

    return {
      onClick,
      isOpen
    }
  }
}
</script>

<template>
  <li class="folder" :class="{'closed': !isOpen}">
    <div ref="label" class="group">
        <div class='text' @click="onClick">
          {{label}}
        </div>
        <ul class="dat-ul indented">
          <slot></slot>
        </ul>
    </div>
  </li>
</template>

<style scoped lang="scss">
@import "../base.scss";

li.folder {
  .text {
    font-weight: bold;
    user-select: none;
    cursor: pointer;
    padding: 5px 5px 5px 16px;
    background: $folder-open;
    text-align: left;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  }

  ul.indented {
    margin-left: $nest-margin;
    width: calc(100% - #{$nest-margin});
  }

  &.closed {
    .text {
      background: $folder-closed;
    }
    ul {
      display: none;
    }
  }
}
</style>
