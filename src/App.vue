<template>
  <div id="app">
    <TextEditor />
  </div>
</template>

<script>
import TextEditor from './components/TextEditor.vue'

export default {
  name: 'App',
  components: {
    TextEditor
  }
}
</script>

<style>
#app {

margin: 0;
padding: 0;
box-sizing: border-box;
font-family: 'Roboto';
}
</style>
