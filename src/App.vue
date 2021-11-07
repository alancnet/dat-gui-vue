<script>
import { ref } from 'vue'
import DatGui from './components/DatGui.vue'
import DatBoolean from './components/DatBoolean.vue'
import DatButton from './components/DatButton.vue'
import DatColor from './components/DatColor.vue'
import DatFolder from './components/DatFolder.vue'
import DatNumber from './components/DatNumber.vue'
import DatSelect from './components/DatSelect.vue'
import DatString from './components/DatString.vue'

export default {
  name: 'App',
  components: {
    DatGui,
    DatBoolean,
    DatButton,
    DatColor,
    DatFolder,
    DatNumber,
    DatSelect,
    DatString
  },
  setup () {
    const open = ref(true)
    const someBool = ref(true)
    const clickCount = ref(0)
    const color = ref('#00ff00')
    const folderOpen = ref(false)
    const someInteger = ref(50)
    const someFloat = ref(0.5)
    const anotherNumber = ref(65535)
    const someChoice = ref('Leonardo')
    const ninjaTurtles = ['Donatello', 'Leonardo', 'Michaelangelo', 'Raphael']
    const someString = ref('Hello World')

    return {
      open,
      someBool,
      clickCount,
      color,
      folderOpen,
      someInteger,
      someFloat,
      anotherNumber,
      someChoice,
      ninjaTurtles,
      someString
    }
  }
}
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <pre>{{({open, someBool, clickCount, color, folderOpen, someInteger, someFloat, anotherNumber, someChoice, someString})}}</pre>
  <DatGui v-model:open="open">
    <DatBoolean label="Some Bool" v-model="someBool" />
    <DatButton label="Increment" @click="clickCount++" />
    <DatColor label="Color" v-model="color" />
    <DatFolder label="Folder" v-model:open="folderOpen">
      <DatBoolean label="Some Bool" v-model="someBool" />
      <DatButton label="Increment" @click="clickCount++" />
      <DatColor label="Color" v-model="color" />
    </DatFolder>
    <DatNumber label="Some Integer" v-model="someInteger" :min="0" :max="100" :step="1" />
    <DatNumber label="Some Float" v-model="someFloat" :min="0" :max="10" :step=".1" />
    <DatNumber label="Another Number" v-model="anotherNumber" />
    <DatSelect label="Some choice" v-model="someChoice" :items="ninjaTurtles" />
    <DatString label="Some String" v-model="someString" />

  </DatGui>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
pre {
  text-align: left;
  padding: 0 calc(50% - 100px);
  max-width: 200px;
}
</style>
