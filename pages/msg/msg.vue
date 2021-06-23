<template>
	<view>
		<template v-if="list.length>0">
			<!-- 信息列表 -->
			<block v-for="(item,index) in list" :key='index'>
				<msg-list :item="item" :index="index"></msg-list>
			</block>
		</template>
		<template v-else>
			<no-thing></no-thing>
		</template>
	</view>
</template>

<script>
	
	const demo=[
		{
			avatar:"/static/default.jpg",
			username:"编程",
			update_time:1624438628,
			data:"内容内容内容让你让人内容内容内容内容内容",
			noread:20
		},
		{
			avatar:"/static/default.jpg",
			username:"编程",
			update_time:1624438628,
			data:"内容内容内容让你让人内容内容内容内容内容",
			noread:20
		},
		{
			avatar:"/static/default.jpg",
			username:"编程",
			update_time:1624437713,
			data:"内容内容内容让你让人内容内容内容内容内容",
			noread:20
		},
		{
			avatar:"/static/default.jpg",
			username:"编程",
			update_time:1624437713,
			data:"内容内容内容让你让人内容内容内容内容内容",
			noread:20
		},
		{
			avatar:"/static/default.jpg",
			username:"编程",
			update_time:1624437713,
			data:"内容内容内容让你让人内容内容内容内容内容",
			noread:20
		}
	]
	
	import msgList from '@/components/msg/msg-list.vue';
    import noThing from '@/components/common/no-thing.vue';
	export default {
		components:{ 
			msgList,
			noThing
		},
		data() {
			return {
				list:[]
			};
		},
		onPullDownRefresh() {
		  console.log(111)
			this.refesh()
		},
		onLoad() {
			this.list=demo
		},
		methods:{
			// 下拉刷新
			refesh(){
				setTimeout(()=>{
					this.list=demo
					// 停止下拉刷新
					uni.stopPullDownRefresh()
				},2000)
			}
		}
		
		
	}
</script>

<style lang="scss">

</style>
