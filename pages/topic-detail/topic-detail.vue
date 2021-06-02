<template>
	<view>
		<view class="">
			<image src="/static/demo/topicpic/1.jpeg" mode="aspectFill" style="height: 300rpx;" class="w-100"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		onLoad(e) {
			console.log(e.detail)
			let res =JSON.parse(e.detail)
			console.log(res)
		}
	}
</script>

<style lang="scss">

</style>
