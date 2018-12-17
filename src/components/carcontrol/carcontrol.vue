<template>
	<div class="carcontrol" >
		<transition enter-active-class="animated fadeInRight" leave-active-class="animated fadeOutLeft">
		<div class="reduce" v-show="food.count>0" @click.stop="reduce">
          <span class="icon-remove_circle_outline"></span>
		</div>
		</transition>
		<transition enter-active-class="animated fadeInRight" leave-active-class="animated adeOutLeft"> 
		<div class="count" v-show="food.count>0">
		{{food.count}}
		</div>
		</transition>
		<div class="add"  @click.stop="add">
		<span class="icon-add_circle"></span>
		</div>
	</div>
</template>

<script>
	export default {
		name: "carcontrol",
		props:["food"],
		data(){
			return {}
		},
		methods:{
			reduce(){
				if(this.food.count>0){
					this.food.count--;
				}
			},
			add(e){
				if(this.food.count>0){
					this.food.count++;
				}else{
					this.$set(this.food,"count",1)
				}
				var target= e.target;
				this.$emit("balladd",target)
				console.log("马上触发事件")
			}
		}

	}
</script>

<style lang="less">
.carcontrol{
	position: absolute;
	right: 0;
	bottom: 5px;
	display: flex;
	span{
		color: rgb(0,160,220);
		font-size: 24px;
	}
	.count{
		color:darkgray;
		padding: 0 5px;
	}
}
</style>