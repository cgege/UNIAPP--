<template>
	<view>
		<!-- 搜索历史 -->
		<view class="py-2 font-md px-2">搜索历史</view>
		<view class="flex flex-wrap ">
			<view class="border rounded font mx-2 my-1 px-2" 
			hover-class="bg-light"
			v-for="(item,index) in list" :key="index">
				{{item}}
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[
					'前端开发框架','让web开发更迅速','Bootstrap','渐进式框架','Kotlin可以编译成Java字节码','java开发的集成环境'
				]
			};
		}
	}
</script>

<style lang="scss">

</style>
