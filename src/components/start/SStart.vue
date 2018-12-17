<template>
	<div class="start">
		<div class="container">
			<div class="content">
				<h1>{{seller.name}}</h1>
				<div class="stars">
					<span class="on" v-for="item in on"></span>
					<span class="half" v-for="item in half"></span>
					<span class="off" v-for="item in off"></span>
				</div>
				<div class="title">
					<div class="line"></div>
					<div class="text">优惠信息</div>
					<div class="line"></div>
				</div>
				<div class="supports">
					<div class="support-item" v-for="data in seller.supports">
						<span class="icon" v-bind:class="classGroup[data.type]"></span>
						<span class="description">{{data.description}}</span>
					</div>
				</div>
				<div class="title">
					<div class="line"></div>
					<div class="text">商家公告</div>
					<div class="line"></div>
				</div>
				
			</div>
		</div>
		<div class="close" @click="startHide">
			<span class="icon-close"></span>
		</div>
	</div>
</template>

<script>
	export default{
		name:"SStart",
		props:["seller"],
		data:function(){
			return {
				on:0,
				half:0,
				off:0,
				classGroup:["decrease","discount","guarantee","invoice","special"]
			}
		},
		methods:{
			startHide:function(){
				this.$parent.start=false;
			}
		},
     created:function(){
     	var num =this.seller.score;
     	this.on = Math.floor(num);
     	if((num-this.on)>=0.5){
     		this.half = 1
     	}
     	this.off=5 - this.on - this.half;
     }
	}
</script>

<style lang="less">
 @import url("../../assets/less/mixin.less");
.start{
	position:fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background-color: rgba(7,17,27,0.8);
	overflow-y:auto;
	z-index:9999;
	.container{
		padding: 64px 36px;
		min-height:100%;
		.content{
			h1{
				font-size: 16px;
				font-weight: 700;
				color: #ffffff;
				line-height: 16px;
				text-align: center;
				margin-bottom: 16px;
			}
			.stars{
				display: flex;
				justify-content: center;
				margin-bottom:28px ;
				span{
					margin: 0 10px;
					width: 20px;
					height: 19px;
					&.on{.bg-img("../../components/start/star48_on")}
					&.half{.bg-img("../../components/start/star48_half")}
					&.off{.bg-img("../../components/start/star48_off")}
				}
			}
			.title{
				display: flex;
				.line{
					flex:3;
				.border-1px;
				border-color: #fff;
				}
				.text{
					flex: 2;
					padding: 0 12px;
					font-size: 14px;
					font-weight: 700;
					line-height: 14px;
					color: #ffffff;
					text-align: center;
					
				}
			}
			.supports{
				margin-bottom: 28px;
				.support-item{
					margin-top: 8px;
					span.icon{
						display: inline-block;
						width: 16px;
						height: 16px;
						margin-right:6px ;
						margin-top: 10px;
					}
					span.description{
						padding-left: 12px;
						margin-bottom: 12px;
						font-size: 16px;
						color: #ffffff;
					}
					/*classGroup:["decrease","discount","guarantee","invoice","special"];*/
					.decrease{
						.bg-img("../../components/start/decrease_1")
					}
					.discount{
						.bg-img("../../components/start/discount_1")
					}
					.guarantee{
						.bg-img("../../components/start/guarantee_1")
					}
					.invoice{
						.bg-img("../../components/start/invoice_1")
					}
					.special{
						.bg-img("../../components/start/special_1")
					}
				}
			}
		}
	}
	               .close{
	               	margin-top: -80px;
	               	text-align: center;
	               	font-size: 32px;
	               	color: rgba(255,255,255,0.5);
	               }
}
</style>