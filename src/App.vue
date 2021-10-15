<template>
  <div id="app">
    <Form />
  </div>
</template>

<script>
import Form from './components/Form.vue'
export default {
  name: 'app',
  components: {
    Form
  }
}
</script>

<style lang="scss">
#app {
  position: absolute;
  top: 50%;
  left: 50%;
  margin: -330px 0 0 -200px;
  z-index: 9999; 
  background: #FFFFFF;
  box-shadow: 0px 12px 24px rgba(44, 39, 56, 0.02), 0px 32px 64px rgba(44, 39, 56, 0.04);
  border-radius: 24px;
  
}
body {
  font: 'IBM Plex Sans', sans-serif;
  background-color: #E5E5E5;
}
</style>
