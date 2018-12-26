<template>
	<div class="viewer-container">
		<div class="header">
			{{item.theme}}
		</div>
		<div>
			<span class="label">От:</span>{{item.from}}
		</div>
		<div>
			<span class="label">Кому:</span>{{item.to}}
		</div>
		<div>
			{{item.date}} {{item.time}}
		</div>
		<div class="buttons">
			<div class="button" @click="$emit('back')">
				Назад
			</div>
				
			<div :class="{disabled:!item.prev}" class="button"  @click="$emit('prev',item)">
				Пред.
			</div>		
			
			<div :class="{disabled:!item.next}" class="button" @click="$emit('next',item)">
				След.
			</div>
		</div>
		<div class="area-container">
			{{item.message}}
		</div>
		<div class="answer-container">
			<textarea v-model="answer" rows="4">					
			</textarea>				
			<div class="button" :class="{disabled:!answer.length}" @click="$emit('answer',answer)">
				Ответить
			</div>
		</div>
		
	</div>
</template>
<script>
	export default {
		name:"mail-viewer",
		props:{
			item:Object
		},
		data(){
			return {
				answer:""
			}
		}
	}
</script>
<style scoped>
	.viewer-container {
		width: calc(100% - 50px);
		height: calc(100% - 32px);
		padding: 16px 25px;
		display:flex;
		flex-flow:column;
	}
	
	.viewer-container > div {
		padding:8px;
		border-bottom: 1px solid grey;
		flex:0 0 auto;
	}
	
	.header {
		font-weight:bold;
		font-size:22px;
	}
	.label{
		font-weight:bold;
	    min-width: 50px;
		display: inline-block;
	}
	.viewer-container .buttons{
		padding-left: 0px;
		padding-right: 0px;
		border-bottom:none;
		display:flex;
		flex-flow:row;
	}
	.viewer-container .area-container{
		flex:1 1 auto;
		margin-top: 25px;
		border-radius: 8px;
		background-color: #f9f9f9;
		box-shadow: inset 0px 0px 8px 0px black;
		border-bottom:none;
			
	}
	.button{
		margin:4px 0px;
		margin-right:8px;
		padding:8px 16px;
		border-radius: 4px;
		background: linear-gradient(to right, rgba(179,221,255,1) 0%,rgba(199,230,255,1) 43%,rgba(217,238,255,1) 97%);
		transition: all .3s;
		box-shadow: 2px 2px 4px 0px #0000007a;
		user-select: none; 	
		cursor:pointer;
	}
	.button.disabled, .button.disabled:hover{
		pointer-events: none;
		background: linear-gradient(to right, rgba(221,221,221,1) 31%,rgba(243,243,243,1) 90%);
	}
	
	.button:hover{
		background: linear-gradient(to right, rgba(255,179,125,1) 13%,rgba(255,199,159,1) 53%,rgba(255,217,189,1) 92%);
		box-shadow: 6px 6px 10px 0px #0000007a;
		
	}
	.viewer-container .answer-container{
		margin:0;
		padding:8px 0px;
		border:none;
	}
	.answer-container{
		display:flex;
		width:100%;
		align-items:flex-start;
		
	}
	.answer-container textarea{
		flex: 1 1 auto;
		margin-right: 15px;
		margin-top: 4px;
		padding:8px;
		outline: none;
		resize: none;
		border-radius: 8px;		
		box-shadow: inset 0px 0px 8px 0px black;
	}
</style>