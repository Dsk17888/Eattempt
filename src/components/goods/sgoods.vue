<template>

	<div class="goods">
		<div class="goods-menu" ref="mscroll">
			<ul class="item-container">
				<li v-for="(item,index) in goods" v-bind:class="{'menu-item':true,'active':index==currentIndex,'v-menu-item':true}" @click=clickIndex(index)>
					<span class="name">
					<span v-if="item.type>=0" v-bind:class="classGroup[item.type]"  class="icon"></span>{{item.name}}</span>
				</li>
			</ul>
		</div>
		<div class="goods-content" ref="gscroll">
			<ul>
				<li v-for="good in goods" class="good v-good">
					<h1>{{good.name}}</h1>
					<ol>
						<li v-for="food in good.foods" class="food" @click="Eselectfood(food)">
							<div class="avatar">
								<img v-bind:src="food.icon" alt="" />
							</div>
							<div class="food-content">
								<h2 class="title">{{food.name}}</h2>
								<div class="description"></div>
								<div class="totle">
									<span>月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="new">￥{{food.price}}</span>
									<span v-if="food.oldPrice" class="old">￥{{food.oldPrice}}</span>

								</div>
							</div>
							<carcontrol v-bind:food="food" v-on:balladd="getball" ></carcontrol>
						</li>
					</ol>
					
				</li>
			</ul>
			
		</div>
        <shopcart v-bind:selectfoods="selectfoods" v-bind:seller="seller" ref="shopcart" ></shopcart>
        <carlist v-bind:selectfoods="selectfoods" ref="carlist" v-show="carshow"></carlist>
	    <food v-bind:selectfood="selectfood" ref="food"></food>
	</div>

</template>

<script>
	import BScroll from 'better-scroll';
	import shopcart from "@/components/shopcart/shopcart"
	import carcontrol from "@/components/carcontrol/carcontrol"
		import data from "@/assets/data/data.json";
		import carlist from "@/components/carlist/carlist"
		import food from "@/components/food/food"
	export default {
		name: "sgoods",
		props:["seller"],
		data: function() {
			return {
				carshow:false,
				goods:[ ],
				classGroup: ["decrease", "discount", "special", "guarantee", "invoice"],
				listHeight: [],
				currentIndex: 0,
				selectfood:[]
			
			}
		},
		components:{
			shopcart,
			carcontrol,
			carlist,
			food
			
			
		},
		computed:{
			selectfoods(){
				var select=[];
				this.goods.forEach(function(good){
						good.foods.forEach(function(food){
							if(food.count>0){
								select.push(food)
							}
						})
					
				})
				return select;
			}
		}
		,
		watch:{
			selectfoods:function(newvalue){
				if(newvalue.length<1){
					console.log(123)
					this.carshow = false;
				}
			}
		},
		
		methods: {
			Eselectfood(food){
				this.selectfood = food;
				this.$refs.food.isshow=true;
			},
			_initScroll() {
				//使用betterscroll插件初始化标签并获取对象
				this.mscroll = new BScroll(this.$refs.mscroll, {
					click: true,
					probeType: 3
				});
				this.gscroll = new BScroll(this.$refs.gscroll, {
					click: true,
					probeType: 3
				}); //使用betterscroll插件初始化标签并获取对象
				//给betterscrll对象（可以理解dom对象）添加事件处理
				this.gscroll.on("scroll", (pos) => {
					//获取滚动距离的绝对值
					var currentHeight = pos.y;
					if(currentHeight<=0){
						currentHeight=-currentHeight;
					};
					currentHeight ++;
					
					for(var i = 0; i < this.listHeight.length; i++) {
						var preHeight = this.listHeight[i];
						var nextHeight = this.listHeight[i + 1]
						if(currentHeight >= preHeight && currentHeight < nextHeight) {
							this.currentIndex = i;
							break;
						}
					}
					//设置左侧菜单滚动到 高亮的 选项
					var currentTag = this.$refs.mscroll.getElementsByClassName("v-menu-item")[this.currentIndex];
					this.mscroll.scrollToElement(currentTag, 300); //betterscrll绑定的标签滚动到指定的字标签位置
				})

			},
			getball(target){
				
				this.$refs.shopcart.ballchange(target);
				
				
			},
			_computedHeight() {
				var height = 0;
				this.listHeight.push(height);
				var lists = this.$refs.gscroll.getElementsByClassName("v-good");
				for(var i = 0; i < lists.length; i++) {
					var liHeight = lists[i].offsetHeight;
					height = height + liHeight;
					this.listHeight.push(height);
					
				}
			},
			clickIndex(index) {
				var lists = this.$refs.gscroll.getElementsByClassName("v-good");
				this.gscroll.scrollToElement(lists[index], 300)
			}
		},
		created: function() {
//			this.$http.get("http://127.0.0.1:3000/api/goods").then((response) => {
//				this.goods = response.body.data;
//
//				
//			})
this.goods=data.goods
this.$nextTick(() => {
					this._initScroll();
					this._computedHeight();

				})


		}
	}
</script>

<style lang="less">
	@import url("../../assets/less/mixin.less");
	.goods {
		position: absolute;
		top: 179px;
		bottom: 50px;
		width: 100%;
		
		display: flex;
		overflow: hidden;
		.goods-menu {
			width: 80px;
			background-color: #F3F5F7;
			.item-container {
				.menu-item {
					width: 60px;
					height: 54px;
					line-height: 14px;
					text-align: center;
					font-size: 12px;
					color: #3A4148;
					padding-left: 12px;
					display: table;
					margin: 0 auto;
					&.active {
						background-color:#fff;
						color: red;
						font-size: 14px;
					}
					.border-1px;
					.name {
						display: table-cell;
						vertical-align: middle;
						.icon {
							display: inline-block;
							width: 12px;
							height: 12px;
							&.decrease {
								.bg-img("../../components/start/decrease_3")
							}
							&.discount {
								.bg-img("../../components/start/discount_3")
							}
							&.guarantee {
								.bg-img("../../components/start/guarantee_3")
							}
							&.invoice {
								.bg-img("../../components/start/invoice_3")
							}
							&.special {
								.bg-img("../../components/start/special_3")
							}
						}
						font-size: 12px;
					}
				}
			}
		}
		.goods-content {
			height: 100%;
			flex: 1;
			.good {
				
				h1 {
					border-left: 2px solid #d9dde1;
					font-size: 12px;
					color: rgb(147, 153, 159);
					font-weight: 700;
					height: 24px;
					line-height: 24px;
					background-color: #f3f5f7;
					padding-left: 14px;
				}
				li.food {
					position: relative;
					display: flex;
					margin: 18px 18px 0;
					padding-bottom: 18px;
					.border-1px;
					border-color: lightgray;
					.avatar {
						width: 67px;
						height: 67px;
						margin-right: 10px;
						img {
							width: 100%;
							height: 100%;
							border-radius: 5px;
						}
					}
					.food-content {
						padding-top: 2px;
						.title {
							font-size: 14px;
							color: rgb(7, 17, 27);
							line-height: 14px;
						}
						.description {
							font-size: 10px;
							color: rgb(147, 153, 159);
							line-height: 10px;
							margin-bottom: 8px;
						}
						.totle {
							font-size: 10px;
							color: rgb(147, 153, 159);
							line-height: 10px;
						}
					}
					.price {
						.new {
							font-size: 14px;
							color: #E74C3C;
							font-weight: 700;
							line-height: 24px;
							margin-right: 8px;
							vertical-align: top;
						}
						.old {
							font-size: 10px;
							color: rgb(147, 153, 159);
							font-weight: 700;
							line-height: 24px;
							text-decoration: line-through;
						}
					}
				}
			}
		}
	}
</style>