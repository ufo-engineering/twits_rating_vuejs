<template>
  <div class="header">
    <h1>Twits <button v-if="logged" class="logout-btn" v-on:click="logout">Logout</button></h1>

  </div>
</template>

<script>
export default {
  name: 'Header',
  props: ['logged', 'logout'],
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass">
  .header
    color: #fff
    background-color: #006597
    text-align: center
    padding: 10px
    .logout-btn
      float: right
      vertical-align: bottom
      margin-top: 5px
      background-color: transparent
      color: #fff
      border: none
      padding: 0
      cursor: pointer
      font-size: 15px
    h1
      margin: 10px 0
      font-size: 22px
      font-weight: normal
</style>
