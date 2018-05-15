<template>
  <div id="app">

    <Header :logged="logged" :logout="this.logout"/>
    <transition  name="slide">
    <div  v-if="logged" key="form">
      <div class="twits-wrp"  v-for="twit in twits" v-bind:key="twit.id" >
        <Twit :twitProp="twit"  :userProp="user_id"/>
      </div>
  </div>
    <div class="form-wrp" v-else key="twitsGrid">
        <form action="" class="login-form" v-on:submit="login">
          <input type="text" class="id-input"><input type="submit" value="send">
        </form>
    </div>
  </transition>
  </div>
</template>

<script>
import Header from './components/Header'
import Twit from './components/Twit'
export default {
  name: 'App',
  components: {
    Header,
    Twit
  },
  data: function(){
    return {
      user_id: null,
      twits: null,
      logged: false,
    }
  },
  created: function(){
    this.getTwits();
    let tween = document.createElement('script');    
        tween.setAttribute('src',"//cdn.jsdelivr.net/npm/tween.js@16.3.4");
        document.head.appendChild(tween);


  },
  methods: {
    login: function(e) {
      e.preventDefault();
      this.logged = true;
      this.user_id = e.currentTarget.querySelector('.id-input').value;
      this.getTwits();
    },
    logout: function(e){
      this.logged = false;
    },
    getTwits: function() {
        let that = this;
        fetch('/static/assets/json/twits.json', {
           headers : {
             'Content-Type': 'application/json',
             'Accept': 'application/json'
            }
        }).then(function(response) {
            response.json().then(data => (that.twits = data))
        }).catch(function(error) {
          console.log('There has been a problem with your fetch operation: ' + error.message);
        });
    }
  }
}
</script>

<style lang="sass">
  @import url('https://fonts.googleapis.com/css?family=Roboto:400,500,700')
  body
    padding: 0
    margin: 0
    font-family: Roboto
    background-color: #fff
  .slide-leave-active,
  .slide-enter-active
    transition: 1s
  .slide-enter
    transform: translate(100%, 0)
  .slide-leave-to
    transform: translate(-100%, 0)

  #app
    overflow: hidden
    min-height: 100vh
    *
      outline: none
    .form-wrp
      position: relative
    .login-form
      background-color: #eaeaea
      display: table
      width: 100%
      max-width: 450px
      left: 50%
      margin: 10% auto 0
      position: absolute
      padding: 25px
      text-align: center
      transform: translateX(-50%)
      input
        display: inline-block
        height: 40px
        padding: 0
        border: none
        vertical-align: top
      input[type="submit"]
        background-color: #006597
        color: #fff
        cursor: pointer
        font-family: Roboto
        text-transform: uppercase
        padding: 0 15px
        font-size: 16px
        font-weight: 500
      .id-input
          width: 200px
          padding-left: 15px
    .twits-wrp
      width: 100%
      max-width: 780px
      margin: 0 auto
</style>
