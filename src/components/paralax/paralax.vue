<template>
	<div :style="style">
	</div>
</template>
<script>
	import back  from "../../assets/prlx-bgnd.jpg"
	export default{
	
		data(){
			return {
				oversize: 0.2,
				w:0,
				h:0,
				offsetX:0,
				offsetY:0,
				x:0,
				y:0
			}
		},
		computed:{
			style(){
			
				let size = (1+this.oversize)*100+"%";
				return {
					background:"url("+back+")",
					position:"fixed",
					top: this.y + "px",
					left: this.x + "px",
					width: size,
					height:size,
					backgroundSize:"cover",
					zIndex:-10
				}
			}
		},
		created(){
			this.initParams();
			window.addEventListener("resize",this.initParams);
			window.addEventListener("mousemove",this.onMouseMove);
		},
		destroyed(){
			window.removeEventListener("resize",this.initParams);
			window.removeEventListener("mousemove",this.onMouseMove);
		},
		methods:{
			initParams(){
				this.w = window.innerWidth;
				this.h = window.innerHeight;
				this.offsetX = window.innerWidth * this.oversize;
				this.offsetY = window.innerHeight * this.oversize; 
			},
			onMouseMove(e){
				
				let x = e.clientX;
				let y = e.clientY;
				
				let partX = x / this.w;
				let partY = y / this.h;
				
				this.x = -this.offsetX*partX
				this.y = -this.offsetY*partY;
				
			}
		}
		
	}
</script>