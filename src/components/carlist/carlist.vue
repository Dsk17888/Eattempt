<template>
	<div class="carlist" >
	 <div class="car-head">
	<div class="title">购物车</div>
	<div class="clear" @click="cleargood">清空</div>
	 </div>
	 <div class="car-content" ref="carcontent">
	 	<transition-group tag="ul" leave-active-class="animated bounceOut" enter-active-class="animated bounceIn">
	 		
	 	
	 		<li class="single-list" v-for="food in selectfoods" v-bind:key="food.name">
	 			<div class="foodname">
	 				{{food.name}}
	 			</div>
	 			<div class="subtotal">
	 				${{food.price*food.count}}
	 			</div>
	 			<carcontrol v-bind:food="food" v-on:balladd="getball" ></carcontrol>
	 		</li>
	 	</transition-group>
	 </div>
		
	</div>
</template>

<script>
	import carcontrol from "@/components/carcontrol/carcontrol"
	import betterScroll from 'better-scroll';
	export default{
		name:"carlist",
		props:["selectfoods"],
		
		data(){
			return {
				
			}
		},
		components:{
			carcontrol
		},
		methods:{
			cleargood(){
				this.selectfoods.forEach((food)=>{
					food.count=0;
				})
				
			},
			getball(target){
					this.$parent.getball(target)
				}
//			getball(){
//				this.$refs.carcontent.ballchange(target);
//			},
//			totalcount(){
//				var total=0;
//				this.selectfoods.forEach((food)=>{
//					total=total+food.count;
//					
//				})
//				return total;
//			}
		},
		mounted(){
			this.$nextTick(()=>{
				console.log(this.$refs.carcontent)
			    new betterScroll(this.$refs.carcontent,{})
					

				})
		
		}
	}
</script>

<style lang="less">
	@import url("../../assets/less/mixin.less");
	
.carlist{
	position: fixed;
	bottom: 50px;
	left: 0;
	max-height: 200px;
	overflow: hidden;
	background-color: #fff;
	width: 100%;
	z-index:7777;
	.car-head{
		width: 100%;
		display: flex;
		padding: 0 18px;
		height: 40px;
		justify-content:space-between;
		box-sizing:border-box;
		background-color:#f3f5f7;
		.border-1px;
		border-color:lightgray;
		.title{
			font-size: 14px;
		line-height: 40px;
		}
		.clear{
			font-size: 12px;
			line-height: 40px;
			color: cornflowerblue;
		}
		
	}
	.car-content{
		overflow: hidden;
		max-height: 160px;
		ul{
			.single-list{
				margin:0 18px;
				height: 48px;
				display: flex;
				justify-content: space-between;
				position: relative;
			.foodname{
				font-size: 14px;
				line-height: 48px;
			}
			.subtotal{
				font-size: 14px;
				line-height: 48px;
				margin-right: 100px;
				color: red;
				font-weight: 700;
			}
			.carcontrol{
				bottom: 10px;
				
			}
			}
		}
	}
}
</style>