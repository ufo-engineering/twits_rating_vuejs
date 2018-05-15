<template>
  <div class="rating" >
	<button class="voice inc" value="1" v-on:click="handleVoiceAction"><span>insightful</span></button>
	
	<transition>
		<div class="gradient" :style="gradient">
			<strong class="rate" :style="ratePosition">{{ratingData}}</strong>
		</div>
	</transition>	
	<button class="voice dec" value="0" v-on:click="handleVoiceAction"><span>noise</span></button>
  </div>
</template>

<script>
// import { TWEEN } from 'gsap';
export default {
	name: 'Rating',
	props: ['rating', 'userId', 'twitId'],
	data: function(){
		return {
			startRating: this.rating,
			ratingData: this.rating 
		}
	},
	computed: {
	  gradient () {
	    return `background: -moz-linear-gradient(left, #006597 0%, #ffffff ${this.ratingData}%);
			background: -webkit-linear-gradient(left, #006597 0%,#ffffff  ${this.ratingData}%);
			background: linear-gradient(to right, #006597 0%,#ffffff  ${this.ratingData}%);`
	  },
	  ratePosition (){
	  	return `left: ${this.ratingData}%`
	  }
	},
	watch: {
    	startRating: function (newValue, oldValue) {
			this.tween(parseInt(oldValue), parseInt(newValue))
    	}
	},
	methods: {
		tween: function (startValue, endValue) {
		  var that = this
		  function animate () {
		    if (TWEEN.update()) {
		      requestAnimationFrame(animate)
		    }
		  }
		  new TWEEN.Tween(startValue)
		    .to(endValue, 500)
		    .onUpdate(function (cc) {
		    	if(startValue < endValue){
		    		that.ratingData = startValue+parseInt((endValue-startValue)*cc)
		    	}
		    	if (startValue > endValue){
		    		that.ratingData = startValue-parseInt((startValue-endValue)*cc)
		    	}
		    })
		    .start()
		  animate()
		},
		handleVoiceAction: function(e){
			let clickedElement = e.currentTarget;
			clickedElement.classList.add('clicked');
			setTimeout(function(){
				clickedElement.classList.remove('clicked')
			},500);
			this.sendVote(clickedElement.value);
		},
		sendVote: function(vote){
			let that = this,
				oldRat;


				console.log()
			
			if(vote == 0){
				if(that.ratingData == 100) return
				oldRat = parseInt(that.ratingData) + 25;
				if(oldRat > 100) oldRat = 100
			}else{
				if(that.ratingData == 0) return
				oldRat = parseInt(that.ratingData) - 25;
				if(oldRat < 0) oldRat = 0
			}
			this.startRating = oldRat;
			// fetch('/twits/api/vote/'+that.twitId, {
			// 	method: "POST",
   //              headers: {
   //                'Accept': 'application/json',
   //                'Content-Type': 'application/x-www-form-urlencoded'
   //              },
			// 	body:  `vote=${vote}&user_id=${that.userId}`
			// }).then(function(response) {
			//     response.json().then(data => {
			//     	that.rating = data.rating;
			//     	// that.ratingData =  data.rating
			//     });
			// }).catch(function(error) {
			//   console.log('There has been a problem with your fetch operation: ' + error.message);
   //        });
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass">
@keyframes effect_dylan
  50%
    transform: scale(1.1, 1.1)
    opacity: 0
  99%
    transform: scale(0.001, 0.001)
    opacity: 0
  100%
    transform: scale(0.001, 0.001)
    opacity: 1
.rating
	width: 100%
	height: 35px
	position: relative
	margin-top: 25px
	display: flex
	flex-direction: row
	align-content: flex-start
	box-shadow: 1px 1px 4px rgba(0,0,0,.4)
	align-items: center
	justify-content: center
	.gradient
		height: 100%
		display: flex
		position: relative
		width: auto
		flex-grow: 1
		transition: all 0.3s ease
	.voice
		height: 35px
		border: none
		width: 120px
		width: 120px
		display: flex
		cursor: pointer
		text-align: center
		background-color: transparent
		color: #fff
		overflow: visible
		font-weight: 600
		position: relative
		text-transform: uppercase
		transition: all 0.3s ease
		&:before
		  content: ''
		  background-color: rgba(0,101,151,.3)
		  // border-radius: 50%
		  display: block
		  position: absolute
		  top: 0
		  width: 100%
		  height: 100%
		  right: 0
		  bottom: 0
		  left: 0
		  transform: scale(0.001, 0.001)
		&.clicked
		  &:before
		    animation: effect_dylan 0.8s ease-out
		span
			display: block
			width: 100%
			height: 100%
			text-align: center
		&:hover
			background-color: rgba(0,0,0,.1)
		&.inc
			left: 0
			background-color: #006597
			&:hover
				background-color: #035882
			&:before
			  background-color: rgba(255,255,255,.3)
		&.dec
			right: 0
			color: #888
			background-color: #fff
			&:hover
				background-color: #f7f7f7
				color: #1D3461
	.rate
		position: absolute
		top: 50%
		height: 40px
        display: none
		z-index: 4
		width: 40px
		background-color: #fff
		text-align: center
		box-shadow: 1px 1px 4px rgba(0,0,0,.4)
		line-height: 40px
		font-size: 14px
		border-radius: 50%
		margin: -20px 0 0 -20px

</style>
