<template>	
	<div class="shopcart" >
		<div class="left">
			<div class="icon" @click="listshow" >
				<span class="icon-shopping_cart car" v-bind:class="{enough:totalcount>0}"></span>
				<span class="emblem" v-if="totalcount>0">
					{{totalcount}}
				</span>
				<transition @before-enter="ballbefore" @enter="ballenter" @after-enter="ballafter">
				<div class="ballouter" v-show="ballshow">
					<div class="ballinner">
						
					</div>
				</div>
				</transition>
			</div>
			<div class="totalprice" v-bind:class="{enough:totalprice>0}">
				￥{{totalprice}}
			</div>
			<div class="deliveryPrice">
			另需配送費{{seller.deliveryPrice}}元
			</div>
			
		</div>
		<div class="right" v-bind:class="{enough:totalprice>=seller.minPrice}">
			<span>{{paycontent}}</span>
		</div>
	</div>
</template>

<script>
	export default {
		name: "shopcart",
		props: ["selectfoods","seller"],
		data() {
			return {
			ballshow:false,
         
          x:0,
          y:0
			}
		},
		methods: {
			listshow(){
				if(this.totalcount>0){
					this.$parent.carshow=!this.$parent.carshow;
				}
			},
           ballchange(target){
           this.ballshow=true;
           
           var rect=target.getBoundingClientRect();
           var x=rect.left;
           var y=rect.top;
           this.x=x-39;
           this.y=    -(window.innerHeight - y -25);
          console.log("被父组件调用 获取点击按钮")
           
           },
           ballbefore(el){
//         	el.style.transform='translateX('+this.x+'px)';
              el.style.transform=`translateX(${this.x}px)`;
              el.style.transition="all 0.4s linear";
              el.style.opacity=1;
              
              var inner=el.getElementsByClassName("ballinner")[0];
              
              inner.style.transform=`translateY(${this.y}px)`;
              inner.style.transition="all 0.4s cubic-bezier(.21,-0.69,.83,.67)";
           },
           ballenter(el,done){
           	el.offsetHeight;
           	 el.style.transform=`translateX(0)`
           	 el.style.opacity=0.3;
              var inner=el.getElementsByClassName("ballinner")[0];
              inner.style.transform=`translateY(0)`;
              done();
           },
           ballafter(){
           	this.ballshow = false;
           }
		},
		
		computed: {
        totalprice(){
        	var total=0;
        	this.selectfoods.forEach((food)=>{
        		total=total+food.price * food.count;
        	})
//      	for( i in selectfoods){
//      		total=total+this.selectfoods[i].price * this.selcetfoods[i].count
//      	}

return total

;

      },
      totalcount(){
      	var total=0;
      	this.selectfoods.forEach((food)=>{
      		total=total+food.count
      		
      	})
      	return total;
      },
        paycontent(){
        	if(this.totalprice=0){
        		return "起送價" +this.seller.minPrice+"元"
        	}else if(this.totalprice<this.seller.minPrice){
        		return "差" +(this.seller.minPrice - this.totalprice) +"起送"
        	}else{
        		return "結算"
        	}
        }
		}
	}
</script>

<style lang="less">
	@import url("../../assets/less/mixin.less");
.shopcart{
	width: 100%;
	display: flex;
	background-color: #141d27;
	position: fixed;
	left: 0;
	bottom: 0;
	justify-content: space-between;
	height: 50px;
	z-index: 8888;
	.left{
		flex: 1;
		display:flex;
		.icon{
			width: 44px;
			height: 44px;
			margin:0 18px;
			position: relative;
			top: -8px;
			background-color: #2b333b;
			border-radius:50%;
			border: 6px #141D27 solid;
			
			span{
				display: inline-block;
				width: 100%;
				height: 100%;
				text-align: center;
				font-size: 24px;
			color: rgba(255,255,255,0.4);
			line-height: 44px;
			
			width: 44px;
			height: 44px;
			border-radius: 50%;
			&.enough{
				background: #449FDB;
				color: #fff;
			}
			}
			.emblem{
				color: #fff;
				font-size: 9px;
				background-color: red;
				width: 24px;
				height: 16px;
				line-height: 16px;
				padding: 0 6px;
				position: fixed;
				bottom: 42px;
				left: 47px;
				border-radius: 8px;
				box-sizing: border-box;
				box-shadow: 0 4px 8px rgba(0,0,0,0.4);
			}
			.ballouter{
				position: fixed;
				bottom: 23px;
				left: 39px;
				width: 16px;
				height: 16px;
				border-radius: 50%;
				
				.ballinner{
					width: 16px;
					height: 16px;
					border-radius: 50%;
					background-color:#00a0dc;
				}
			}
		}
		.totalprice{
			margin-top: 8px;
			height: 36px;
			padding-right: 12px;
			font-size: 16px;
			color: rgba(255,255,255,0.4);
			line-height: 36px;
			font-weight: 700;
			text-align: center;
			.border-1px;
			border-bottom: none;
			border-right-style: solid;
			border-right-color:rgba(255,255,255,0.1);
			&.enough{
				
			}
		}
		.deliveryPrice{
			font-size: 16px;
			font-weight: 700;
			line-height: 50px;
			color: rgba(255,255,255,0.4);
			margin-left: 12px;
			
		}
	}
	.right{
		width: 105px;
		box-sizing: border-box;
		padding: 0 8px;
		line-height: 50px;
		background-color: #2b333b;
		color:rgba(255,255,255,0.4); ;
		&.enough{
				background-color: #4B986A;
				color: #FFFFFF !important;
			}
	   span{
	   	
	   	
	   	font-size: 12px;
	   	line-height: 24px;
	   	font-weight: 700;
	   
	   }
	  
	  
	}
}

</style>