

<template>
	<div class="fooddetail" v-show="isshow"  ref="food">
		<div>
		    <div class="back-icon" >
		    	<span class="icon-close" @click="back"></span>
		    </div>
			<div class="food-img">
				<img v-bind:src="selectfood.image"/>
			</div>
			<div class="foodinfo">
				<div class="title">
					{{selectfood.name}}
				</div>
				<div class="sellcount">
					<span class="monthsell">
						月售{{selectfood.sellCount}}份
					</span>
					<span class="goodrate">
						好评率：{{selectfood.rating}}%
					</span>
				</div>
				<div class="price">
					<span class="newp"  v-bind:class="{special:selectfood.oldPrice}">
						￥{{selectfood.price}}
					</span>
					<span class="oldp" v-if="selectfood.oldPrice">
						￥{{selectfood.oldPrice}}
						
					</span>
				</div>
				<div class="addin">
					<transition enter-active-class="animated fadeInDown">
					<span class="addcar" @click="addfood" v-show="!selectfood.count">加入购物车</span>
					</transition>
					<transition enter-active-class="animated fadeInDown">
					<carcontrol v-bind:food="selectfood" v-show="selectfood.count"></carcontrol>
					</transition>
				</div>
			</div>
			<div class="foodins">
				<div class="title">商品介绍</div>
				<p class="information">{{selectfood.info}}</p>
			</div>
			<div class="detail-ratings">
				<div class="title">
					商品评价
				</div>
				<div class="choose-rating">
					<span class="all" @click="choose(2)" v-bind:class="{active:this.choosen==2}">全部</span>
					<span class="uprate" @click="choose(1)" v-bind:class="{active:this.choosen==1}">推荐</span>
					<span class="downrate" @click="choose(0)" v-bind:class="{active:this.choosen==0}">吐槽</span>
				</div>
				<div class="choose-count" @click="Ehascontent">
					<span class="icon-arrow_lift istext">只看有内容的评论</span>
					
				</div>
			</div>
			<div class="rating-content">
				<ul class="rating-list">
				<li class="rating-per" v-for="rating in chooseratings" v-bind:key="rating.rateTime">
					<div class="rating-timeuser">
						<span class="time">{{rating.rateTime | time}}</span>
						<span class="user">{{rating.username}}<img class="avatar" v-bind:src="rating.avatar"/></span>
					</div>
					<div class="rantingtext">
						<span v-if="rating.rateType==0" class="icon-thumb_up good"></span>
						<span v-else="" class="icon-thumb_down bad"></span>
						{{rating.text}}
					</div>
				</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
	import betterScroll from 'better-scroll';
	import carcontrol from '@/components/carcontrol/carcontrol';
	export default{
		name:"food",
		props:["selectfood"],
		data(){
			return {
				isshow:false,
				hascontent:false,
				choosen:2,
			}
		},
		components:{
			carcontrol
		},
		methods:{
			choose(num){
				this.choosen = num;
				
			},
			addfood(){
				this.$set(this.selectfood,"count",1)
			},
			back(){
		this.isshow=false;
			},
			Ehascontent(){
				this.hascontent = !this.hascontent;
				
			}
		},
		filters:{
			time(value){
				return new Date(value).toLocaleString()
			}
		},
		computed:{
			allratings(){
				if(this.hascontent==false){
					return this.selectfood.ratings;
				}else if(this.hascontent==true){
					var list=[];
					this.selectfood.ratings.forEach((rating)=>{
						if(rating.text){
							list.push(rating)
						}
					})
				}
				
				return list;
				
			},
			chooseratings(){
				if(this.choosen==2){
					return this.allratings;
					
				}else if(this.choosen==0){
					var list=[];
					this.allratings.forEach((rating)=>{
						if(rating.rateType==1){
							list.push(rating)
						}
					})
					return list;
				}else if(this.choosen==1){
					var list=[];
					this.allratings.forEach((rating)=>{
						if(rating.rateType==0){
							list.push(rating)
						}
					})
					return list;
				}
			}
		},
		mounted(){
			this.$nextTick(()=>{
				if(!this.food){
					this.food = new betterScroll(this.$refs.food);
				}else{
					this.food.refresh();
				}
				
			})
		}
	}
</script>

<style lang="less">
	@import url("../../assets/less/mixin.less");
.fooddetail{
	position: fixed;
	width: 100%;
	top: 0;
	bottom: 50px;
	background-color: rgba(0,0,0,.2);
	background-color: #f3f5f7;
	text-align: left;
	overflow:hidden;
	.back-icon{
		position: absolute;
		top: 10px;
		left: 10px;
		padding: 10px;
		z-index: 1;
		color: white;
		
	}
	.food-img{
		width:100%;
		height: 0;
		padding-top: 100%;
		img{
			position: fixed;
			top: 0;
			left: 0;
			width: 100%;
			
			
		}
	}
	.foodinfo{
		position: relative;
		background-color: #fff;
		padding:0 18px 18px 18px;
		.border-1px;
		border-color:rgba(7,17,24,.1);
		.title{
			font-size: 14px;
			font-weight: 700;
			line-height: 14px;
			padding-top: 18px;
		}
		.sellcount{
			font-size: 10px;
			color: rgb(147,153,159);
			line-height: 10px;
			padding-top: 8px;
			.monthsell{
				
			}
			.goodrate{
				
			}
		}
		.price{
			padding-top: 18px;
			.newp{
				font-size: 10px;
				color: rgb(240,20,20);
				font-weight: normal;
				line-height: 24px;
				&.special{
					font-size: 14px;
					font-weight: 700;
				}
			}
			.oldp{
				font-size: 10px;
				color: rgb(147,153,159);
				font-weight: normal;
				line-height: 24px;
			}
		}
		.addin{
			.addcar{
				display: block;
				width: 74px;
				height: 24px;
				border-radius: 12px;
				background-color:rgb(0,160,220);
				font-size: 10px;
				line-height: 12px;
				color: white;
				padding: 6px 12px;
				box-sizing: border-box;
				position: absolute;
				right: 36px;
				bottom: 15px;
			}
			.carcontrol{
				right: 36px;
				bottom: 15px;
			}
		}
	}
	.foodins{
		
		margin-top: 16px;
		background-color: #fff;
		padding:0 18px 18px 18px;
		border-top: solid rgba(7,17,24,.1);
		border-bottom-color:rgba(7,17,24,.1) !important;
		.border-1px;
		.title{
			font-size: 14px;
			font-weight: 700;
			line-height: 14px;
			padding-top: 18px;
		}
		.information{
			font-size: 12px;
			padding-left: 8px;
			padding-top: 6px;
			line-height: 24px;
			color: rgb(77,85,93);
		}
	}
	.detail-ratings{
		margin-top:18px ;
		background-color: #fff;
		padding:0 18px 0px 18px;
		.border-1px;
			border-color:rgba(7,17,24,.1);
			border-top-style:solid;
			
		.title{
			font-size: 14px;
			font-weight: 700;
			line-height: 14px;
			padding-top: 18px;
		}
		.choose-rating{
			padding: 6px 0 18px;
			.border-1px;
			border-color:rgba(7,17,24,.1);
			.all{
				margin-right: 8px;
				display: inline-block;
				padding: 8px 12px;
				background-color: rgba(0,160,220,0.2);
				font-size: 12px;
				border-radius: 2px;
				color: rgb(77,85,93);
				&.active{
					background-color: rgba(0,160,220,1);
					color: white;
				}
			}
			.uprate{
				margin-right: 8px;
				display: inline-block;
				padding: 8px 12px;
				background-color: rgba(0,160,220,0.2);
				font-size: 12px;
				border-radius: 2px;
				color: rgb(77,85,93);
				&.active{
					background-color: rgba(0,160,220,1);
					color: white;
				}
			}
			.downrate{
				margin-right: 8px;
				display: inline-block;
				padding: 8px 12px;
				background-color: rgba(77,85,93,0.2);
				font-size: 12px;
				border-radius: 2px;
				color: rgb(77,85,93);
				&.active{
					background-color: rgba(77,85,93,0.6);
					color: black;
				}
			}
			
		}
		.choose-count{
			padding: 12px 0;
			.istext{
				font-size: 12px;
				line-height: 24px;
				color: rgb(147,153,159);
			}
		}
	}
	.rating-content{
		padding: 0 18px;
		background-color: #fff;
		.rating-list{
			.rating-per{
				.border-1px;
				border-color:rgba(7,17,24,.1);
				padding: 16px 0;
				.rating-timeuser{
					display:flex;
					justify-content:space-between;
				.time{
					font-size:10px ;
					line-height: 12px;
					color: rgb(147,153,159);
				}
				.user{
					font-size:10px ;
					line-height: 12px;
					color: rgb(147,153,159);
					.avatar{
						width: 12px;
						height: 12px;
						border-radius: 50%;
						margin-left: 6px;
					}
				}
				}
				.rantingtext{
					padding-top: 6px;
					text-indent: 4px;
					font-size: 12px;
					line-height: 16px;
					.bad{
						color: rgb(147,153,159);
						font-size: 12px;
						line-height: 24px;
					}
					.good{
						color: rgb(0,160,220);
						font-size: 12px;
						line-height: 24px;
					}
				}
			}
		}
	}
}
</style>