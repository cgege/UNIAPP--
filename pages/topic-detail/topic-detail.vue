<template>
	<view>
		<topiic-info :info="info"></topiic-info>
		<divider></divider>
		<view class="p-2 flex align-center border-bottom" hover-class="bg-light">
			<text class="iconfont icon-xihuan text-main text-ellipsis"></text>
			<text class="font text-dark">
				【新人必读】 uni-app实战社区开发
			</text>
		</view>
		<view class="p-2 flex align-center border-bottom" hover-class="bg-light">
			<text class="iconfont icon-xihuan text-main "></text>
			<text class="font text-dark text-ellipsis">
				【新人必读】 uni-app实战微信开发
			</text>
		</view>
	</view>
</template>

<script>
	import topiicInfo from '@/components/topiic-detail/topiic-info.vue';
	export default {
		components:{
			topiicInfo
		},
		data() {
			return {
				info:{
					cover:"/static/demo/topicpic/1.jpeg",
					title:"话题名称",
					desc:"话题描述",
					today_count:0,
					news_count:10,
				}
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
	.filter{
		filter: blur(10px);
	}
</style>
