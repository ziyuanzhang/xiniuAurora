<template>
	<view @click="$emit('click')">
		<button><slot></slot></button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
		
		}
	}
</script>

<style>
	button{
		color: #fff;
		height: 100upx;
		line-height: 100upx;
		/* #ifdef H5 */
		animation:mymove  2s   infinite; /* Opera */
			background: linear-gradient(to bottom, #f72030 0%, #ff5428 100%);
		/* #endif */
		border-radius: 30upx;
	

	}
	/* #ifdef H5 */
	@keyframes mymove{
		0%   {transform: scale(1);}
		30%  {transform: scale(0.9);}
		55%  {transform: scale(1);}
		80% {transform: scale(0.9);}
		100% {transform: scale(1);}
		transform: scale(.8);
	}
	/* #endif */

</style>
