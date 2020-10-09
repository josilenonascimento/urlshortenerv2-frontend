<template>
  <header>
    <h1>URL Shortener v2</h1>
  </header>
  <Form />
</template>

<script>
import Form from './components/Form';

export default {
  components: {
    Form,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}

h1 {
  color: #03045E;
  font-size: 2.2rem;
}
</style>
