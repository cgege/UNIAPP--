<template>
	<view>
		<view class=" flex align-center py-2">
			<view
				class="flex-1 flex align-center justify-center "
				v-for="(item, index) in tabBars"
				:key="index"
				:class="index === tabIndex ? 'font-lg text-main font-weight-bold ' : 'font-md '"
				@click="changeTab(index)"
			>
				{{ item.name }}
				<text class="ml-2" v-if="item.mun > 0">{{ item.mun }}</text>
			</view>
		</view>
		<!-- 列表 -->
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab" :style="'height:' + scrollH + 'px;'">
			<swiper-item v-for="(item, index) in newList" :key="index">
				<scroll-view scroll-y="true" :style="'height:' + scrollH + 'px;'" @scrolltolower="loadmore(index)">
					<!-- 文章列表 -->
					<template v-if="item.list.length > 0">
						<block v-for="(item2, index2) in item.list" :key="index2">
							<!-- 列表样式 -->
							<view class="p-2 flex align-center border-bottom border-light-secondary">
								<image style="height: 100rpx; width: 100rpx;" class="rounded-circle mr-2" src="@/static/default.jpg" mode=""></image>
								<view class="flex flex-column flex-1">
									<text class="font-md text-dark">昵称</text>
									<uni-badge text="24" type="error" size="small">
										<text class="iconfont icon-nv text-white font-sm " style="margin-right: 5rpx;"></text>
									</uni-badge>
								</view>
								<view class="uni-icon uni-icon-checkbox-filled text-light-muted"></view>
							</view>
							
						</block>
						<!-- 上拉加载 -->
						<loab-moer :loadmore="item.loadMore"></loab-moer>
						
					</template>
					<template v-else>
						<no-thing></no-thing>
					</template>
				</scroll-view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
import loabMoer from '@/components/common/loab-moer.vue';
import nothing from '@/components/common/no-thing.vue';
import uniBadge from '@/components/uni-ui/uni-badge/uni-badge.vue';
export default {
	components: {
		loabMoer,
		nothing,
		uniBadge
	},
	data() {
		return {
			scrollH: 600,
			tabIndex: 0,
			newList: [],
			tabBars: [
				{
					name: '互关',
					mun: 0
				},
				{
					name: '关注',
					mun: 2
				},
				{
					name: '粉丝',
					mun: 20
				}
			]
		};
	},
	// 点击输入框事件
	onNavigationBarSearchInputClicked() {
		uni.navigateTo({
			url: '../search/search'
		});
	},
	onNavigationBarButtonTap() {
		uni.navigateBack({
			delta: 1
		});
	},
	onLoad() {
		uni.getSystemInfo({
			success: res => {
				this.scrollH = res.windowHeight - uni.upx2px(101);
				// console.log(this.scrollH)
			}
		});
		this.getDate();
	},
	methods: {
		getDate() {
			var arr = [];
			for (let i = 0; i < this.tabBars.length; i++) {
				let obj = {
					list: [],
					loadMore: '上拉加载更多...'
				};
				if (i < 2) {
					obj = {
						list: [1, 2, 3],
						loadMore: '上拉加载更多...'
					};
				}
				arr.push(obj);
			}

			this.newList = arr;
		},
		changeTab(index) {
			this.tabIndex = index;
		},
		// 监听滑动
		onChangeTab(e) {
			this.changeTab(e.detail.current);
		},
		loadmore(index) {
			//数据验证
			if (this.newList[index].loadMore !== '上拉加载更多...') {
				return;
			}
			this.newList[index].loadMore = '加载中...';
			setTimeout(() => {
				this.newList[index].list = [...this.newList[index].list, ...this.newList[index].list];
				this.newList[index].loadMore = '上拉加载更多...';
			}, 2000);
		}
	}
};
</script>

<style lang="scss"></style>
