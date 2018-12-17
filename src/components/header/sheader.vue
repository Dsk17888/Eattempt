<template>
	<div v-if="seller" class="header">
		<div class="content-container">
			<div class="avatar clearfix">
				<img v-bind:src="seller.avatar" alt="" width="64" height="64"/>
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
				<span>{{seller.description}}</span>
				/
				<span>{{seller.deliveryTime}}</span>
				分钟送达
				</div>
				<div class="supports">
					<span class="icon"></span>
					<span>{{seller.supports[0].description}}</span>
				</div>
			</div>
			<div class="supports-count">
				<span>{{seller.supports.length}}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<div class="bulletin" @click="startShow">
			<span class="icon"></span>
			<span class="text">{{seller.bulletin}}</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		<div class="background">
			<img v-bind:src="seller.avatar" alt=""/>
		</div>
		<sstart v-show="start" v-bind:seller="seller"></sstart>
	</div>
</template>

<script>
	import sstart from "@/components/start/SStart";
	export default{
		name:"sheader",
		props:["seller"],
		components: {
			sstart
			
		},
		data:function(){
			return {
				start:false
			}
		},
		methods:{
			startShow:function(){
				this.start=true
			},
			startHide:function(){
				this.start=false
			}
		}
	}
</script>

<style lang="less">
	@import url("../../assets/less/mixin.less");
	.header{
		position: relative;
		text-align:left;
		background-color:rgba(7,17,27,0.5);
		color: rgba(7,17,27,0.5);
		blur:10px;
		
		.content-container{
			position: relative;
			padding: 24px 12px 18px 24px;
			.avatar{
				margin-right: 12px ;
				width: 64px;
				height: 64px;
				float: left;
				img{
					border-radius: 10px;
				}
				}
			.content{
				.title{
					font-size: 16px;
					color: #fff;
					color: rgb(255,255,255);
					font-weight: bold;
					line-height: 18px;
					margin: 2px 0 8px 0;
					.brand{
						display: inline-block;
						width: 30px;
						height: 18px;
						.bg-img("../../components/header/brand")
					}
					.name{
						margin-left: 6px;
					}
				}
				.description{
					margin-bottom: 10px;
					font-size: 12px;
					color: #fff;
					font-weight: 200;
					line-height: 12px;
					
				}
				.supports{
					.icon{
						display: inline-block;
						width: 14px;
						height: 14px;
						.bg-img("../../components/header/decrease_1")
					}
				}
				
			}
			.supports-count{
				position: absolute;
				height: 24px;
				line-height: 24px;
				right: 12px;
				bottom: 12px;
				border-radius:8px ;
				padding: 0 8px;
				font-size: 10px;
				color: #ffffff;
				font-weight: 200;
				
				background-color: rgba(0,0,0,0.2);
				i{
					position: relative;
					top: 1px;
				}
			}
		}
		.bulletin{
			height: 28px;
			line-height: 28px;
			padding: 0 12px;
			font-size: 10px;
			color: #FFFFFF;
			font-weight: 200;
			overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
				position: relative;
			background-color: rgba(7,17,27,0.5);
			.icon{
				display: inline-block;
				vertical-align: middle;
				width: 22px;
				height: 12px;
				line-height: 28px;
				.bg-img("../../components/header/bulletin");
				margin-right: 4px;
			}
			.text{margin-right: 5px;}
			i{
				position: absolute;
				top: 10px;
				right: 5px;
			}
		}
		.background{
			position: absolute;
			width: 100%;
			height: 100%;
			top: 0;
			left: 0;
			z-index: -10;
			img{
				width: 100%;
				height: 100%;
				filter: blur(5px);
			}
		}
	}
</style>