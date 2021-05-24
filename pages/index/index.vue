<template>
	<view>
		<!-- 顶部选项卡 -->
		<scroll-view scroll-x="true" :scroll-into-view="scrollInto" scroll-with-animation class="scroll-row">
			<view
				class="scroll-row-item px-3 py-1 font-md"
				v-for="(item, index) in tablist"
				:key="index"
				:id="'tab' + index"
				:class="tabIndex === index ? 'text-main font-lg font-weight-bold' : ''"
				@click="changeTab(index)"
				style="height: 60rpx">
				{{ item.name }}
			</view>
		</scroll-view>
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab" :style="'height:' + scrollH + 'px;'"   >
			<swiper-item v-for="(item, index) in newsList" :key="index">
				<scroll-view scroll-y :style="'height:' + scrollH + 'px;'" @scrolltolower="loadmore(index)">
					<block v-for="(item2, index2) in item.list" :key="index2">
						<!-- 列表样式 -->
						<common-list :item="item2" :index="index2" @follow="follow"
                @doSupport="doSupport"></common-list>
						<!-- 分割线 -->
						<divider></divider>
					</block>
					<!-- 上拉加载 -->
					<view class="flex align-center justify-center py-3 font">
						<text class="font text-light-muted">{{item.loadmore}}</text>
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
import commonList from '@/components/common/common-list.vue';
export default {
	components: {
		commonList
	},
	data() {
		return {
			scrollH: 800,
			// 顶部选项卡
			scrollInto: '',
			tabIndex: 0,
			tablist: [{ name: '关注' }, { name: '推荐' }, { name: '体育' }, { name: '热点' }, { name: '财经' }, { name: '娱乐' }, { name: '财经' }, { name: '娱乐' }],
			newsList: []
		};
	},
	onLoad() {
		uni.getSystemInfo({
			success: res => {
				this.scrollh=res.windowHeight-uni.upx2px(101)
			}
		})
		// 根据选项生成列表
		this.getData()
	},
	methods: {
		// 获取数据
		getData(){
			var arr=[];
			for(let i=0; i<this.tablist.length;i++){
				// 生成列表模板
				let obj={
					// 1上拉加载更多 2加载中... 3没有更多了
					loadmore:"上拉加载更多",
					list:[{
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
								username: '昵称1',
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
								share_num: 2
							}]
				}
				arr.push(obj)
			}
			this.newsList=arr
		},
		follow(e) {
		  this.newList[0].list[e].isFollow = !this.newList[0].list[e].isFollow;
		  if (!this.newList[0].list[e].isFollow) {
		    uni.showToast({
		      title: "已取消关注",
		    });
		  } else {
		    uni.showToast({
		      title: "关注成功",
		    });
		  }
		  console.log(this.newList[0].list[e]);
		},
		doSupport(e) {
		  let item = this.newList[0].list[e.index];
		  console.log();
		  let msg = e.type === "support" ? "点赞" : "反对";
		  if (item.support.type === "") {
		    item.support.type = e.type;
		    item.support[e.type + "_count"]++;
		  }
		  if (item.support.type === "support" && e.type === "unsupport") {
		    item.support.support_count++;
		    item.support.unsupport_count--;
		  } else if (item.support.type === "unsupport" && e.type === "support") {
		    item.support.support_count--;
		    item.support.unsupport_count++;
		  }
		  item.support.type = e.type;
		  uni.showToast({
		    title: msg + "成功",
		  });
		},
		// 切换选项卡
		changeTab(index) {
			if (this.tabIndex === index) {
				return;
			}
			this.tabIndex = index;
			// 滚动到指定区域
			this.scrollInto = 'tab' + index;
		},
		// 监听滑动
		onChangeTab(e) {
			this.changeTab(e.detail.current);
		},
		
		loadmore(index){
			this.newsList[index].loadmore='加载中...'
			setTimeout(()=>{
				this.newsList[index].loadmore='上拉加载更多'
			},2000)
		}
	}
};
</script>

<style></style>
