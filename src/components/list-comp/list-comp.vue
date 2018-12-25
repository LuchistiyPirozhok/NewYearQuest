<template>
	<div>
		<div style="padding:0;margin:0;height:8px;" class="item">
		</div>
		<div :class="{selected:currentItem==item,item:true}"
			 v-for="item in items" 
			 @click="onClick(item)">
			{{item.name}}  <template v-if="!item.hideCapcity">({{item.items.length}})</template>
			<div v-if="item.hasNew" class="marker"></div>
		</div>
	</div>
</template>
<script>
	export default {
		props:{
			items:{
				type:Array,
				default:()=>({})
			}
		},
		data(){
			return {
				currentItem:null
			}		
		},
		methods:{
			onClick(item){
				this.currentItem = item;
				this.$emit("item-clicked",item);			
			}
		}
	
	}
	
</script>
<style scoped>
	.item {
		padding: 15px;
		border-bottom: 1px solid darkgrey;
		transition: all .3s;
		position:relative;
	}
	.item:hover{
		transform:scale(1.02);
		background: rgb(255,179,125); /* Old browsers */
		background: -moz-linear-gradient(left, rgba(255,179,125,1) 13%, rgba(255,199,159,1) 53%, rgba(255,217,189,1) 92%); /* FF3.6-15 */
		background: -webkit-linear-gradient(left, rgba(255,179,125,1) 13%,rgba(255,199,159,1) 53%,rgba(255,217,189,1) 92%); /* Chrome10-25,Safari5.1-6 */
		background: linear-gradient(to right, rgba(255,179,125,1) 13%,rgba(255,199,159,1) 53%,rgba(255,217,189,1) 92%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
		filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#ffb37d', endColorstr='#ffd9bd',GradientType=1 );
		box-shadow: 3px 2px 20px rgba(0,0,0,0.5);
		border-bottom: 1px solid transparent;
		z-index:1;
	}
	
	.item.selected {
		background: rgb(179,221,255); /* Old browsers */
		background: -moz-linear-gradient(left, rgba(179,221,255,1) 0%, rgba(199,230,255,1) 43%, rgba(217,238,255,1) 97%); /* FF3.6-15 */
		background: -webkit-linear-gradient(left, rgba(179,221,255,1) 0%,rgba(199,230,255,1) 43%,rgba(217,238,255,1) 97%); /* Chrome10-25,Safari5.1-6 */
		background: linear-gradient(to right, rgba(179,221,255,1) 0%,rgba(199,230,255,1) 43%,rgba(217,238,255,1) 97%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
		filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#b3ddff', endColorstr='#d9eeff',GradientType=1 );
	}
	
	.marker{
		position: absolute;
		right: 5px;
		top: calc(50% - 4px);
		width: 8px;
		height: 8px;
		border-radius: 50%;
		background: #e81a1a;
	}
</style>