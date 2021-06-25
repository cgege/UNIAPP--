<template>
	<view>
		<template v-if="list.length>0">
			<!-- 信息列表 -->
			<block v-for="(item,index) in list" :key='index'>
				<msg-list :item="item" :index="index" ></msg-list>
			</block>
		</template>
		<template v-else>
			<no-thing></no-thing>
		</template>
		
		<!-- 弹出层-->
		<uni-popup ref="popup" type="top" >
			<view class="bg-white flex justify-center align-center font-md border-bottom py-2 " hover-class="bg-light" @click="popupEvent('friend')">
				<text class="iconfont icon-sousuo mr-2"></text>添加好友
			</view>
			<view class="bg-white flex justify-center align-center font-md border-bottom py-2" hover-class="bg-light" @click="popupEvent('cleak')">
				<text class="iconfont icon-shanchu mr-2"></text>清除列表
			</view>
			
		</uni-popup>
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
	import uniPopup from '@/components/uni-ui/uni-popup/uni-popup.vue';
	export default {
		components:{ 
			msgList,
			noThing,
			uniPopup
		},
		data() {
			return {
				list:[]
			};
		},
		// 监听原生导航栏按钮
		onNavigationBarButtonTap(e) {
			switch(e.index){
				case 0:
				uni.navigateTo({
					url: '../user-list/user-list',
					
				});
				this.$refs.popup.close()
					break;
				case 1:
				this.$refs.popup.open()
					break;	
			}
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
			},
			popupEvent(e){
				switch (e){
					case 'friend':
					
						break;
					case 'cleak':
					console.log(123)
						break;
				}
				this.$refs.popup.close()
			}
		}
		
		
	}
</script>

<style lang="scss">

</style>
