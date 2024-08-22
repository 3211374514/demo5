<template>
	<view class="touxiang">
		<!-- 人员基本资料卡片 -->
		<u-card :show-head="false" :thumb="thumb" box-shadow="1rpx 1rpx 1rpx 1rpx rgba(0, 0, 0, 0.1)" border="false">

			<view class="" slot="body" border="false" v-model="infoList">
				<!-- 头像 -->
				<view class="tou">
					<u-avatar :src="infoList.src" show-sex="true" :sex-icon="infoList.sex" size="large"></u-avatar>
				</view>
				<!-- 姓名 -->
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title-name u-line-2">{{infoList.name}}</view>
				</view>
				<!-- 工号 -->
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title-gonghao u-line-2">{{infoList.gonghao}}</view>
				</view>
				<!-- 目前在岗工位 -->
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title-gongwei u-line-2">{{infoList.gongwei}}</view>
					<u-tag text="工位" size="mini" mode="plain" type="warning" />
				</view>
				<!-- 技能掌握总数 -->
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<view class="u-body-item-title-jineng u-line-2">技能掌握：{{infoList.skillnum}}</view>
				</view>
			</view>

		</u-card>
		<!-- 分段器 -->
		<u-subsection :list="list" :current="curNow" @change="sectionChange"></u-subsection>
		<!-- <u-subsection :list="list" :current="1"></u-subsection> -->
		<!-- 个人技能显示 -->
		<view class="collapse" v-if="curNow==0">
			<u-collapse :accordion="false">
				<u-collapse-item :title="item.head" v-for="(item, index) in itemList" :key="index">
					<template v-slot:title>
						<view class="title-container">
							<text>{{item.head}}</text>
							<!-- 进度条 -->
							<u-line-progress active-color="#2979ff" :percent="(item.point/4)*100" class="progress"
								:striped="true" :striped-active="true"></u-line-progress>
							<!-- 星星评价 -->
							<u-rate :count="4" v-model="item.point" class="starpoint" :colors="starColors"
								disabled></u-rate>
						</view>
					</template>
					<view>
						技能等级：{{item.point}}/4
					</view>
					<view>
						等级认证时间：{{item.time}}
					</view>
				</u-collapse-item>
			</u-collapse>
			<view class="button01" v-if="showButton==1 && curNow==0" @click="changeShowButton">
				<u-button type="primary" shape="square" :ripple="true"
					@click="jumpToINewPages('info2','')" >修改信息</u-button>
				<u-button type="success" shape="square" :ripple="true"
					@click="jumpToINewPages('addskillinfo','')">添加技能</u-button>
				<u-button type="error" shape="square" :ripple="true"
					@click="jumpToINewPages('delskillinfo','')">删除技能</u-button>
			</view>
		</view>
		<!-- 工作经历显示 -->
		<view class="work" v-if="curNow==1">
			<u-cell-item :arrow="false" icon="setting-fill" :title="item.gongduan" :value="item.gongwei"
				:label="item.time" v-for="(item, index) in workitemlist" :key="index"></u-cell-item>
			
			<view class="button01" @click="changeShowButton">
				<u-button type="primary" shape="square" :ripple="true"
					@click="jumpToINewPages('info3','')">修改工作经历</u-button>
				<u-button type="success" shape="square" :ripple="true"
					@click="jumpToINewPages('addworkinfo','')">添加工作经历</u-button>
				<u-button type="error" shape="square" :ripple="true"
					@click="jumpToINewPages('delskillinfo','?curNow=1')">删除工作经历</u-button>
			</view>
		</view>
		<!-- 评价显示 -->
		<view class="work" v-if="curNow==2">
			<view class="desc-style">
				<u-alert-tips type="success" :description="description" :desc-style="descStyle"></u-alert-tips>
				<u-button type="primary" shape="square" :ripple="true"
					@click="jumpToINewPages('info3','?curNow=1')">修改评价</u-button>
			</view>
			
		</view>
		<!-- 底部按钮 -->
		<view class="button01" v-if="showButton==0" @click="changeShowButton">
			<u-button type="warning" shape="square" size="medium" :ripple="true"
				@click="changeShowButton">警告按钮</u-button>
		</view>




	</view>
</template>


<script>
	export default {
		data() {
			return {

				showButton: 0,

				descStyle: {
					fontSize: "30rpx"

				},

				description: '松下问童子，言师采药去。只在此山中，云深不知处。',

				workitemlist: [{
					gongduan: "涂装C线上胶工段",
					gongwei: "CS44R",
					time: "2024.08.19",
				}, {
					gongduan: "涂装C线打磨工段",
					gongwei: "CS44R",
					time: "2023.06.10",
				}, {
					gongduan: "涂装C线上胶工段",
					gongwei: "CS44R",
					time: "2024.08.19",
				}],

				curNow: 0,

				itemStyle: {
					marginTop: '20px'
				},
				starColors: ['#ffc454', '#ff8408', '#ff4c05'],
				starIcons: ['thumb-down-fill', 'thumb-down-fill', 'thumb-up-fill', 'thumb-up-fill'],

				itemList: [{
					head: "赏识在于角度的转换",
					point: 1,
					time: "只要我们正确择取一个合适的参照物乃至稍降一格去看待他人，值得赏识的东西便会扑面而来",
					open: true,
					disabled: true
				}, {
					head: "生活中不是缺少美，而是缺少发现美的眼睛",
					point: 2,
					time: "学会欣赏，实际是一种积极生活的态度，是生活的调味品，会在欣赏中发现生活的美",
					open: false,
				}, {
					head: "周围一些不起眼的人、事、物，或许都隐藏着不同凡响的智慧",
					point: 4,
					time: "但是据说雕刻大卫像所用的这块大理石，曾被多位雕刻家批评得一无是处，有些人认为这块大理石采凿得不好，有些人嫌它的纹路不够美",
					open: false,
				}],

				infoList: {
					src: 'https://tse3-mm.cn.bing.net/th/id/OIP-C.BQLPis_OmX8ELLZDQNjiaQAAAA?rs=1&pid=ImgDetMain',
					sex: "man",
					name: "张三",
					gonghao: "22400111",
					gongwei: "上胶工段43R",
					skillnum: "3",
				},


				list: [{
						name: '个人技能'
					},
					{
						name: '工作经历'
					},
					{
						name: '评价'
					}
				],
				current: 1,

				text: '无头像',
				title: '素胚勾勒出青花，笔锋浓转淡',
				subTitle: '2020-05-15',
				thumb: 'http://pic2.sc.chinaz.com/Files/pic/pic9/202002/hpic2119_s.jpg',
			}
		},
		onLoad() {

		},
		methods: {

			jumpToINewPages(page,data) {
				// 使用 uni.navigateTo 跳转到目标页面
				uni.navigateTo({
					url: '/pages/' + page + '/' + page+data, // 目标页面路径

					success: () => {
						console.log('跳转成功');
					},
					fail: (err) => {
						console.error('跳转失败', err);
					},
				});
			},

			changeShowButton() {
				this.showButton = (this.showButton + 1) % 2;
				console.log(this.showButton);
			},

			sectionChange(index) {
				this.curNow = index;
				console.log(this.curNow);
			}

		}
	}
</script>

<style>
	.button01 {
		/* text-align:center; */
		/* position: fixed; */
		bottom: 10%;
		left: auto;
		right: auto;
		text-align: center;
		display: flex;
		flex-direction: row;
		align-items: center;
	}

	.progress {
		margin-top: 8rpx;
		width: 400rpx;
	}

	.title-container {
		display: flex;
		flex-direction: column;

	}

	.u-body-item-title-gonghao {
		font-size: 40rpx;
		color: #00aaff;
	}

	.u-body-item-title-jineng {
		font-size: 40rpx;
		color: #666;
	}

	.u-body-item-title-name {
		font-size: 60rpx;
	}

	.u-card-wrap {
		background-color: $u-bg-color;
		padding: 1px;
	}

	.u-body-item {
		display: flex;
		font-size: 32rpx;
		color: #333;
		padding: 20rpx 10rpx;
		align-items: center;
		justify-content: center;
	}

	.u-body-item image {
		width: 120rpx;
		flex: 0 0 120rpx;
		height: 120rpx;
		border-radius: 8rpx;
		margin-left: 12rpx;
	}

	.tou {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>