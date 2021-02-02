<template>
	<view style="width: 100%;position: relative;overflow: hidden;" :style="{height:heights+'px'}" >
		<view :class="retain?'mask-mess':[!retain && moveDistance>=this.heights/3?'mask-show':'']" style="background-color: #4CD964;width: 100%;height: 60%;position: absolute;" :style="{bottom:(heights-moveDistance)+'px'}" @tap.stop="">1236999</view>
		<view :class="retain?'contentk':[!retain && moveDistance>=this.heights/3?'contentk-show':'']" style="background-color: #007AFF;width: 100%;height: 100%;position: absolute;" :style="[moveDistance>0?{top:moveDistance+'px'}:'']" ></view>
		<view :class="retain?'contentk':[!retain && moveDistance>=this.heights/3?'contentk-show':'']" class="slider" style="width: 40%;height: 160rpx;background-color: #F0AD4E;position: absolute;left: 30%;" 
		:style="[moveDistance>0?{top:moveDistance+'px'}:'']"  @touchmove.stop.prevent="touchMove" @touchstart.stop.prevent="touchStart" @touchend.stop.prevent="touchEnd"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				heights:0,
				start:0,
				move:0,
				moveDistance:0,
				end:0,
				retain:false,
				pageLeft:0,
				pageRight:0,
				pageTop:0,
				initMmess:{},
				initStatus:false
			}
		},
		onLoad() {
           this.heights  = uni.getSystemInfoSync().windowHeight;
		   this.getNode();
		},
		methods: {
			test(){
				// this.moveDistance = 0;
				// this.initPage();
				console.log('test')
			},
		    touchStart(e){
				console.log('tochStart',e);
				// console.log('==>',this.retain)
				this.moveDistance = 0;
				this.initPage();
				if(!this.isAllow(e.changedTouches[0].pageY,e.changedTouches[0].pageX))return;
				this.start = e.changedTouches[0].pageY;
				this.retain = false;
				// if()
		   },
           touchMove(e){
			   console.log('tochMove',e);
			   this.getNode();
			   if(!this.isAllow(e.changedTouches[0].pageY,e.changedTouches[0].pageX)){
				   this.initPage();
				   return;
			   };
			   this.move = e.changedTouches[0].pageY;
			   this.moveDistance = this.move - this.start;
			   console.log('tochMove',this.moveDistance);
		   },
		   touchEnd(e){
			    console.log('touchEnd',e);
				if(!this.isAllow(e.changedTouches[0].pageY,e.changedTouches[0].pageX))return;
				this.end = e.changedTouches[0].pageY;
				this.moveDistance = this.end - this.start;
				console.log('touchEnd',this.moveDistance);
				this.initPage()
		   },
		   isAllow(y,x){
			   if(y<this.pageTop && (x>=this.pageLeft && x<=this.pageRight)){
			   	  return true;
			   }else{
				   return false;
			   }
		   },
		   getNode(){
			   this.$nextTick(function(){
			   	uni.createSelectorQuery().select('.slider').boundingClientRect(data=>{
			   			console.log('slider',data);
						if(!this.initStatus){
							this.initMmess = data;
							this.initStatus=true;
							this.pageLeft = data.left;
							this.pageRight = data.right;
							this.pageTop = data.height;
						}else{
							this.pageTop = data.top + this.initMmess.height;
						}
			   	}).exec();
			   })
		   },
		   initPage(){
			   if(this.moveDistance> Math.floor(this.heights/3)){
			   	this.retain = false;
			   }else{
			   	this.retain = true;
			   	this.moveDistance = 0;
			   	this.move =0;
			   	this.start = 0;
			   	this.end = 0;
			   }
		   }
		}
	}
</script>

<style>
	.contents{
		position: absolute;
		top:100rpx;
		left:0;
		transition: 0.5s ease-out;
	}
	.contentk{
		position: absolute;
		top:0 !important;
		left:0;
		transition: 0.5s ease-out;
	}
	.contentk-show{
		position: absolute;
		top:80% !important;
		left:0;
		transition: 0.5s ease-out;
	}
	.mask-mess{
		position: absolute;
		left: 0;
		bottom:100% !important;
		transition: 0.5s ease-out;
	}
	.mask-show{
		position: absolute;
		left: 0;
		bottom:20% !important;
		transition: 0.5s ease-out;
	}
</style>
