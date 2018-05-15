<template>
  <div class="twit">
    <p>{{ twitProp.content }}</p>
    <Rating :rating="twitProp.rating" :userId="userProp" :twitId="twitProp.id" />
  </div>
</template>

<script>
import Rating from './Rating'
export default {
  name: 'Twit',
  components: {
    Rating
  },
  props: ['twitProp', 'userProp']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass">
  .twit
    padding: 25px
    margin: 30px 0
    border-bottom: 1px solid #ADCAD6
    color: #1D3461
    font-size: 14px
</style>
