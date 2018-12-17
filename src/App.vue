<template>
  <div id="app">
  	<sheader v-bind:seller="seller"></sheader>
  	<div class="tab">
  		
  		<div class="item">
  			<router-link to="/">商品</router-link>
  		</div>
  		<div class="item">
  			<router-link to="/ratings">评价</router-link>
  		</div>
  		<div class="item">
  			<router-link to="/seller">商家</router-link>
  		</div>
  	</div>
  
  <router-view  v-if="seller" v-bind:seller="seller"></router-view>
  </div>
</template>


<script>
	import sheader from "@/components/header/sheader";
	import sgoods from "@/components/goods/sgoods";
	import data from "@/assets/data/data.json";
	
	export default {
		name: 'App',
		data:function(){
			return {seller:null}
		},
		components: {
			sheader,
			sgoods
			
		},
		created:function(){
			//then()是使用ES6 的promise() 封装的一个方法
			//vue-cli 自带了promise()的封装代码，提供了一个then()接受回调函数
//			this.$http.get("http://127.0.0.1:3000/api/seller").then(function(response){
//				if(response.body.errMsg==0){
//					this.seller = response.body.data;
//									}
//			})
this.seller=data.seller;
		}
	}
</script>

<!--less这些预处理器语言，支持正常css代码-->
<!--设置style中的语法为less语法-->
<style lang="less">
	/*通过@import导入外部的css文件*/
	@import url("assets/css/icon.css");
	@import url("assets/less/mixin.less");
#app {
  text-align: center;
  
  .tab{
  	display:flex;
  	height:40px;
  	line-height:40px;
  	
  	
  	.item{
  		flex: 1;
  		.border-1px;
  		a{
  			display:block;
  			height: 40px;
  			font-size: 14px;
  			color: rgb(77,85,93);
  		}
  		a.router-link-exact-active{
  			color:rgb(240,20,20)
  		}
  	}
  }
}
</style>
