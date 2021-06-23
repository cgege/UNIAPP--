<template>
	<view>
		<topiic-info :info="info"></topiic-info>
		<divider></divider>
		<view
			class="p-2 flex align-center border-bottom"
			hover-class="bg-light"
			v-for="(item, index) in hotlist"
			:key="index"
		>
			<text class="iconfont icon-xihuan text-main text-ellipsis"></text>
			<text class="font text-dark">{{ item.title }}</text>
		</view>
		<!-- tab -->
		<view class=" flex align-center py-2">
			<view
				class="flex-1 flex align-center justify-center "
				v-for="(item, index) in tabBars"
				:key="index"
				:class="index === tabIndex ? 'font-lg text-main font-weight-bold ' : 'font-md '"
				@click="changeTab(index)"
			>
				{{ item.name }}
			</view>
		</view>
		<!-- 列表 -->
		<template v-if="listData.length > 0">
			<block v-for="(item, index) in listData" :key="index">
				<common-list :item="item" :index="index"></common-list>
				<divider></divider>
			</block>
			<!-- 上拉加载 -->
			<loab-moer :loadmore="loadtext"></loab-moer>
		</template>
		<template v-else>
			<no-thing></no-thing>
		</template>
	</view>
</template>

<script>
const demo = [
	{
		username: '昵称1',
		userpic: '/static/default.jpg',
		newstime: '2020-05-24 上午 11：31',
		isFollow: false,
		title: '这是一个标题',
		titlepic: '/static/demo/banner2.jpg',
		support: {
			type: 'support',
			support_count: 1,
			unsupport_count: 5
		},
		comment_count: 4,
		share_num: 2
	},
	{
		username: '昵称2',
		userpic: '/static/default.jpg',
		newstime: '2020-05-24 上午 11：31',
		isFollow: false,
		title: '这是一个标题',
		titlepic: '/static/demo/banner2.jpg',
		support: {
			type: 'unsupport',
			support_count: 1,
			unsupport_count: 5
		},
		comment_count: 4,
		share_num: 0
	},
	{
		username: '昵称1',
		userpic: '/static/default.jpg',
		newstime: '2020-05-24 上午 11：31',
		isFollow: false,
		title: '这是一个标题',
		titlepic: '/static/demo/banner2.jpg',
		support: {
			type: '',
			support_count: 0,
			unsupport_count: 0
		},
		comment_count: 4,
		share_num: 2
	}
];
import topiicInfo from '@/components/topiic-detail/topiic-info.vue';
import commonList from '@/components/common/common-list.vue';
import noThing from '@/components/common/no-thing.vue';
import loabMoer from '@/components/common/loab-moer.vue';
export default {
	components: {
		topiicInfo,
		commonList,
		noThing,
		loabMoer
	},
	data() {
		return {
			tabIndex: 0,
			tabBars: [
				{
					name: '默认'
				},
				{
					name: '最新'
				}
			],
			info: {
				cover: '/static/demo/topicpic/1.jpeg',
				title: '话题名称',
				desc: '话题描述',
				today_count: 0,
				news_count: 10
			},
			hotlist: [
				{
					title: '【新人必读】 uni-app实战微信开发'
				},
				{
					title: '【新人必读】 uni-app实战微信开发'
				}
			],
			list1: [],
			loadtext1: '上拉加载更多',
			list2: [],
			loadtext2: '上拉加载更多'
		};
	},
	computed: {
		listData() {
			return this['list' + (this.tabIndex + 1)];
		},
		// 当前上拉加载
		loadtext() {
			if (this.tabIndex === 0) {
				return this.loadtext1;
			}
			return this.loadtext2;
		}
	},
	onLoad(e) {
		let res = JSON.parse(e.detail);

		this.list1 = demo;
	},
	// 触底事件
	onReachBottom() {
		this.loadmore();
	},
	methods: {
		changeTab(index) {
			this.tabIndex = index;
		},
		loadmore() {
			let index = this.tabIndex;
			if (this.loadtext !== '上拉加载更多') return;
			//加载中
			this['loadtext' + (this.tabIndex + 1)] = '加载中...';
			//请求数据
			setTimeout(() => {
				this['list' + (index + 1)] = [...this['list' + (index + 1)], ...this['list' + (index + 1)]];
				this['loadtext' + (index + 1)] = '上拉加载更多...';
			}, 2000);
		}
	}
};
</script>

<style lang="scss">
.filter {
	filter: blur(10px);
}
</style>
