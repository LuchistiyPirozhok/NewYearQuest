<template>
	<div class="table-container">
		<table v-if="items.length" class="table">
			<thead>
				<th style="width:15%">
				</th>
				<th>			
				</th>
				<th style="width:15%">				
				</th>
			</thead>
			<tbody>
				<tr v-for="item in items" @click="onClick(item)" class="item">
					<td>
						{{item.from}}
					</td>
					<td>
						{{item.message}}
					</td>
					<td>
						{{item.time}}
					</td>
				</tr>
			</tbody>
		</table>
		<div v-else :style="{backgroundImage:'url('+empty+')'}" class="placeholder"></div>
	</div>
	
</template>
<script>
	import empty from "../../assets/empty.png"
	export default {
		props:{
			items:{
				type:Array,
				default:()=>({})
			}
		},
		data(){
			return {
				empty,
				currentItem:null
			}
		},
		methods:{
			onClick(item){
				this.currentItem=item;
				this.$emit("item-clicked",item);
			}
		}
	}
</script>
<style scoped>
    .placeholder{
		    background-size: cover;
			background-position-x: 50%;
			width: 100%;
			height: 100%;
	}
	.table-container{
		width: calc(100% - 50px);
		height: calc(100% - 32px);
		padding: 16px 25px;
	}
	.table{
		border-collapse: collapse;
	}
	
	.table td{
		border:none;
		padding:15px;
		white-space:nowrap;
		overflow:hidden;
		border-collapse: collapse;
	}
	.table tr{
		border-bottom:1px solid grey;
		border-collapse: collapse;
	}
	.table{
		width:100%;
		padding:15px;

	}
	.item {		
		transition: all .3s;
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

</style>