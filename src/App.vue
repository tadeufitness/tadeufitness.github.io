<template>
  <div id="app">
    <md-tabs md-sync-route md-alignment="centered">
      <md-tab md-label="Home" to="/" exact></md-tab>
      <md-tab md-label="IMC" to="/imc" exact></md-tab>
    </md-tabs>
    <router-view id="main" class="md-layout md-gutter md-alignment-center-center"></router-view>
    <!-- Bottom Bar
    <md-bottom-bar md-sync-route md-type="fixed" md-theme="bottom-bar-blue" id="bottom-bar">
      <md-bottom-bar-item to="/" exact md-label="Home" md-icon="home"></md-bottom-bar-item>
      <md-bottom-bar-item to="/photos" exact md-label="Photos" md-icon="photo"></md-bottom-bar-item>
      <md-bottom-bar-item to="/favorites" exact md-label="Favorites" md-icon="favorite"></md-bottom-bar-item>
    </md-bottom-bar> -->
  </div>
</template>

<style scoped lang="sass">
#main
  min-height: 92vh
  margin: auto
  padding: 15px
</style>

<script>
export default {
  name: 'App'
}
</script>
