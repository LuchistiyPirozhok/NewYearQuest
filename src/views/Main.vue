<template>
	<div class="outer">
		<paralax></paralax>
		<div class="container">
			<div class="header">
				<div>
				</div>
				<img  class="pog" :src="pogChamp"></img>
			</div>
			<div class="content">
				<div class="side-bar">
					<list-comp :items="menuItems" @item-clicked="onMenuClick"></list-comp>
				</div>
				<div class="main-block">
					<div v-if="!selectedMails" class="placeholder"
						:style="{backgroundImage:'url('+image+')'}">						
					</div>
					<mails-comp v-else-if="!currentMail" :items="selectedMails" @item-clicked="onMailClick"></mails-comp>
					<mail-viewer v-else 
								 :item="currentMail"
								 @next="currentMail=$event.next"
								 @prev="currentMail=$event.prev"
								 @back="currentMail=null"
								 @answer="handleAnswer"
								 >						
					</mail-viewer>
				</div>			
			</div>
		</div>	
	</div>	
</template>
<style scoped>
	.placeholder{
		    background-size: cover;
			background-position-x: 50%;
			width: 100%;
			height: 100%;
	}

	.outer{
		height: 100%;
		display: flex;
		justify-content: center;
		flex-flow: row;
	}
	.container {
		width:80%;
		height: 100%;
		display: flex;
		flex-flow: column;
		box-shadow: 0px 0px 20px 16px #0000007a;
	}
	
	.header{
		height: 120px;
	
		position: relative;
		box-shadow: 0px 0px 15px -2px black;
		z-index: 2;
	}
	.header>div{
		background: linear-gradient(to right, rgba(143,143,143,1) 0%, rgba(173,173,173,1) 24%, rgba(189,189,189,1) 73%, rgba(227,224,227,1) 100%);
		width:calc(100% + 1px);
		height:100%;
	}
	
	.content {
		flex:1 1 auto;
		display: flex;
		flex-flow: horizontal;
		align-items:stretch;
	}
	.pog{
		position:absolute;
		top:10px;
		left:10px;
		width:250px;
	}
	
	.side-bar {
		width: 250px;
		flex: 0 0 auto;
		background: rgb(221,221,221); /* Old browsers */
		background: -moz-linear-gradient(left, rgba(221,221,221,1) 31%, rgba(243,243,243,1) 90%); /* FF3.6-15 */
		background: -webkit-linear-gradient(left, rgba(221,221,221,1) 31%,rgba(243,243,243,1) 90%); /* Chrome10-25,Safari5.1-6 */
		background: linear-gradient(to right, rgba(221,221,221,1) 31%,rgba(243,243,243,1) 90%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
		filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#dddddd', endColorstr='#f3f3f3',GradientType=1 );
		box-shadow: 0 0 16px 0px #000000c7;
		z-index: 1;
	}
	
	.main-block {
		flex: 1 1 auto;
		background:#dbdbdb;
	}
	
</style>

<script>
	import pogChamp from "../assets/pog.png"
	import image from "../assets/tree.png"
	import listComp from "../components/list-comp/list-comp.vue"
	import mailsComp from "../components/mails-comp/mails-comp.vue"
	import paralax from "../components/paralax/paralax.vue"
	import mailViewer from "../components/mail-viewer/mail-viewer.vue"
	import * as moment from "moment"
	
	
	
	
	let scenario = [
		{
			theme:"Test1",
			from:"Lorem",
			to:"Ipsum",
			date:"31.12.2018",
			time:"15:36",
			message:"Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
			validate(mail){
				return mail=="test"
			},
			tips:["tip1"],
			idx:0
		}	
	];
	
	let wrong = [
		"Да нет же..."
	];
	
	let inbox = [];
	let outbox = [];
	let spam = [];
	let removed = [];
	
	let inboxMenu = {
		name:"Входящие",
		hasNew:false,
		items:inbox
	};
	let outboxMenu = {
		name:"Исходящие",
		hasNew:false,
		items:outbox
	};
	let spamMenu = {
		name:"Спам",
		hasNew:false,
		items:spam,
		hideCapcity:true						
	};
	let removedMenu = {
		name:"Корзина",
		hasNew:false,
		items:removed
	};
	
	setTimeout(function(){
		inbox.push(scenario[0]);
	},1000);
	
	
	
	export default {
		components:{
			listComp,
			mailsComp,
			paralax,
			mailViewer
		},
		methods:{
			onMenuClick(event){
				if(this.selectedMails==event) return;
				this.selectedMails=event.items;
				this.currentMail=null;
			},
			onMailClick(event){
				this.currentMail=event;
			},
			handleAnswer(answer){
				let current = this.currentMail;
				let outboxMsg = this.createMsg(current,answer);
				
				outbox.push(outboxMsg);
				this.$set(current,"next",outboxMsg);
				
				let valid = current.validate(answer);
				let nextMsg;
				if(valid){
					nextMsg = scenario[current.idx+1];
				}else{
					let tips = current.tips;
					if(tips.length){								  
						nextMsg = this.createMsg(outboxMsg, tips.splice(0,1)[0]);
					}else{
						let idx = Math.floor(Math.random() * wrong.length);
						nextMsg = this.createMsg(outboxMsg, wrong[idx]);
					}
					nextMsg.validate = current.validate;
					nextMsg.tips = current.tips;
					nextMsg.idx = current.idx;
					
				}
				let that = this;
				setTimeout(()=>{
					that.$set(outboxMsg,"next",nextMsg);
					that.$set(nextMsg,"prev",outboxMsg);
					inbox.push(nextMsg);
					inboxMenu.hasNew = true;
					
				},1000);
				
				
			},
			createMsg(current,answer){
				let now = new Date();
				let msg = {
					theme:current.theme,
					from:current.to,
					to:current.from,
					date:moment(now).format("DD.MM.YYYY"),
					time:moment(now).format("HH:mm"),
					message:answer,
					prev:current
				}
				return msg;
			}
		},
		data(){
			return{
				image,
				pogChamp,
				selectedMails:null,
				currentMail:null,
				menuItems:[
					inboxMenu,
					outboxMenu,
					spamMenu,
					removedMenu
				],
				
			}
		}
	}
</script>
