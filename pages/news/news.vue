<template>
	<view>
		<uni-nav-bar :border="false" :fixed="true" :statusBar="true" @click-right="openAddInput">
			<view class="flex align-center justify-center font-weight-bold w-100">
				
				<view class="mx-1" @click="changeTab(index)" 
				v-for="(item,index)  in tabBars" :key="index" 
				:class="tabIndex===index?'font-lg text-main':'text-light-muted'">{{item.name}}</view>
			
			</view>
			<text slot="right" class="iconfont icon-fatie_icon"></text>
		</uni-nav-bar>
		
		<!-- 滑块 -->
		<swiper
		  :duration="150":style="'height:' + scrollH + 'px;'">
		  <swiper-item>
			  <!-- 话题 -->
		  	<scroll-view scroll-y="true" :style="'height:'+scrollH+'px;'" @scrolltolower="loadMoreEvent()">
				
		  		<block v-for="(item,index) in list " :key="index">
		  			<common-list :item="item" :index="index" @doSupport="doSupport"></common-list>
		  			<divider></divider>
		  		</block>
		  		<load-more :loadmore="loadMore"></load-more>
		  	</scroll-view>
		  </swiper-item>
		  <!-- 话题 -->
		  <swiper-item >
		    <scroll-view scroll-y="true" :style="'height:' + scrollH + 'px;'">
					<!-- 热门分类 -->
		  			<hot-cate :hotCate="hotCate"></hot-cate>
					<!-- 搜索框 -->
					<view class="p-2">
						<view class="bg-light rounded flex align-center justify-center py-2 text-secondary">
							<text class="iconfont icon-sousuo mr-2"></text>
							搜索话题
						</view>
					</view>
					<!-- 轮播图 -->
					<swiper class="px-2 pb-2" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
						<swiper-item>
							<image src="@/static/demo/banner2.jpg" style="height: 300rpx; " class="w-100 rounded"></image>
						</swiper-item>
					</swiper>
					<divider></divider>
					<!-- 最近更新 -->
					<view class="p-2 font-md">最近更新</view>
					<!-- 话题列表组件 -->
					<block v-for="(item,index) in topicList" :key="index">
						<topic-list :item="item" :index="index"></topic-list>
					</block>
		    </scroll-view>
		  </swiper-item>
		</swiper>
	</view>
	
</template>

<script>
	const demo = [
	  {
	    username: "昵称1",
	    userpic: "/static/default.jpg",
	    newstime: "2020-05-24 上午 11：31",
	    isFollow: true,
	    title: "这是一个标题",
	    titlepic: "/static/demo/banner2.jpg",
	    support: {
	      type: "support",
	      support_count: 1,
	      unsupport_count: 5,
	    },
	    comment_count: 4,
	    share_num: 2,
	  },
	  {
	    username: "昵称2",
	    userpic: "/static/default.jpg",
	    newstime: "2020-05-24 上午 11：31",
	    isFollow: true,
	    title: "这是一个标题",
	    titlepic: "/static/demo/banner2.jpg",
	    support: {
	      type: "unsupport",
	      support_count: 1,
	      unsupport_count: 5,
	    },
	    comment_count: 4,
	    share_num: 0,
	  },
	  {
	    username: "昵称1",
	    userpic: "/static/default.jpg",
	    newstime: "2020-05-24 上午 11：31",
	    isFollow: true,
	    title: "这是一个标题",
	    titlepic: "/static/demo/banner2.jpg",
	    support: {
	      type: "",
	      support_count: 0,
	      unsupport_count: 0,
	    },
	    comment_count: 4,
	    share_num: 2,
	  },
	];
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	import commonList from '@/components/common/common-list.vue';
	import loadMore from '@/components/common/loab-moer.vue';
	import hotCate from '@/components/news/hot-cate.vue';
	import topicList from '@/components/news/topic-list.vue';
	export default {
		components: {
			uniNavBar,
			commonList,
			loadMore,
			hotCate,
			topicList
		},
		data() {
			return {
				scrollH:500,
				tabIndex:0,
				tabBars:[
					{
						name:"关注"
					},
					{
						name:"话题"
					}
				],
				list:[],
				loadMore: '上拉加载更多...',
				hotCate:[
					{
						name:"关注"
					},
					{
						name:"推荐"
					},
					{
						name:"体育"
					}
				],
				topicList:[
					{
						cover:"/static/demo/topicpic/1.jpeg",
						title:"话题名称",
						desc:"话题描述",
						today_count:0,
						news_count:10,
						
					},
					
					{
						cover:"/static/demo/topicpic/1.jpeg",
						title:"话题名称",
						desc:"话题描述",
						today_count:0,
						news_count:10,
						
					},
					{
						cover:"/static/demo/topicpic/1.jpeg",
						title:"话题名称",
						desc:"话题描述",
						today_count:0,
						news_count:10,
						
					},
					{
						cover:"/static/demo/topicpic/1.jpeg",
						title:"话题名称",
						desc:"话题描述",
						today_count:0,
						news_count:10,
						
					},
					
					
					
				]
				
			};
		},
		onLoad() {
		  uni.getSystemInfo({
		    success: res => {
		      this.scrollH = res.windowHeight - res.statusBarHeight-44;
		     
		    },
		  })
		  // 加载测试数据
		  this.list =demo
		  
		},
		methods:{
			// 打开分布页
			openAddInput(){
				uni.navigateTo({
					uni:'../add-input/add-input'
				})
			},
			changeTab(index){
				this.tabIndex=index
			},
			// 滑动
			onChangeTab(e){
				this.tabIndex=e.detail.current
			},
			// 顶踩操作
			doSupport(e) {
			  let item = this.list[e.index];
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
			// 上拉加载更多
			loadMoreEvent() {
				//数据验证
				if (this.loadMore !== "上拉加载更多...") return;
				this.loadMore = "加载中...";
				
				setTimeout(() => {
					this.list = [...this.list, ...this.list];
					this.loadMore = '上拉加载更多...';
				}, 2000)
			}
		}
	}
</script>

<style lang="scss">

</style>
