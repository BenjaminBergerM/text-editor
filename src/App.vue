<template>
  <div id="app">
    <Header/>
    <main>
      <TextEditor/>
    </main>
  </div>
</template>

<script>
import Header from './components/Header'
import TextEditor from './components/TextEditor'

export default {
  name: 'App',
  components: {
    Header,
    TextEditor
  }
}
</script>

<style>
#app {
  display: flex;
  height: 100%;
  flex-direction: column;
  font-family: 'Roboto', sans-serif;
}
main {
  padding: 0px 40px 0px 40px;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}
</style>
