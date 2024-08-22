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
		<!-- 工作经历显示 -->
		<view class="work" v-if="curNow==0">
			<!-- 修改信息表单 -->
			<u-form>
				<!-- 技能 -->
				<view v-for="(item, index) in workitemlist" :key="index">
					<u-form-item :label="item.gongduan" label-position="top" :label-style="labelStyle">
						<view class="calendarAndbutton2">
							<view>工段：</view><u-input v-model="item.gongduan" />
							<view>岗位：</view><u-input v-model="item.gongwei" />
						</view>

						<view class="calendarAndbutton">
							<view>日期：{{item.time}}</view>
							<u-calendar v-model="item.calendarShow" mode="date"
								@change="changeDate(index,$event)"></u-calendar>
							<u-button @click="open(index)" type="primary" shape="circle" size="mini">选择日期</u-button>
						</view>
					</u-form-item>
				</view>

			</u-form>

			<!-- 提交按钮 -->
			<view>
				<u-button @click="submit" type="warning">提交修改</u-button>
			</view>
			<u-gap height="80" bg-color="#ffffff"></u-gap>



		</view>

		<view v-if="curNow==1">
			<u-input v-model="description" type="textarea" :border="border" height="100" auto-height="true" />

			<!-- 提交按钮 -->
			<view>
				<u-button @click="submit" type="warning">提交修改</u-button>
			</view>
			<u-gap height="80" bg-color="#ffffff"></u-gap>
		</view>





	</view>
</template>

<script>
	export default {
		data() {
			return {

				description: '松下问童子，言师采药去。只在此山中，云深不知处。',

				labelStyle: {
					color: "#00aaff",
				},

				workitemlist: [{
					gongduan: "涂装C线上胶工段",
					gongwei: "CS44R",
					time: "2024.08.19",
					calendarShow: false,
				}, {
					gongduan: "涂装C线打磨工段",
					gongwei: "CS44R",
					time: "2023.06.10",
					calendarShow: false,
				}, {
					gongduan: "涂装C线上胶工段",
					gongwei: "CS44R",
					time: "2024.08.19",
					calendarShow: false,
				}],

				curNow: 0,

				itemStyle: {
					marginTop: '20px'
				},


				list: [

					{
						name: '修改工作经历'
					},
					{
						name: '修改评价'
					},

				],

				infoList: {
					src: 'https://tse3-mm.cn.bing.net/th/id/OIP-C.BQLPis_OmX8ELLZDQNjiaQAAAA?rs=1&pid=ImgDetMain',
					sex: "man",
					name: "张三",
					gonghao: "22400111",
					gongwei: "上胶工段43R",
					skillnum: "3",
				},



			}
		},

		onLoad(option) {
			const curNow = option.curNow;
			this.curNow = option.curNow
			
			console.log('传递过来的参数', curNow);
			console.log('this.curNow', this.curNow);


		},
		
		methods: {

			submit() {

				console.log("workitemlist:", this.workitemlist);
				console.log("description:", this.description);
			},

			sectionChange(index) {
				this.curNow = index;
				console.log(this.curNow);
			},

			open(index) {
				this.workitemlist[index].calendarShow = true;
				console.log(index);
			},

			changeDate(index, e) {
				this.workitemlist[index].time = e.result;
				console.log(e);
				console.log(this.workitemlist);
			},

		}
	}
</script>

<style>
	.buttonSubmit {
		display: flex;
		justify-content: center
	}

	.calendarAndbutton {

		display: flex;
		/* 使用Flexbox布局 */
		justify-content: space-between;
		/* 使文字和按钮两端对齐 */
		align-items: center;
		margin-right: 50rpx;

	}

	.calendarAndbutton2 {

		display: flex;
		/* 使用Flexbox布局 */
		justify-content: space-between;
		/* 使文字和按钮两端对齐 */
		align-items: center;
		margin-left: auto;
		margin-right: auto;


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