<template>
	<view class="info2">
		<!-- 修改信息表单 -->
		<u-form :model="infoList" ref="uForm">
			<u-form-item label="姓名" prop="name"><u-input v-model="infoList.name" /></u-form-item>
			<view class="">
				<u-form-item label="性别"><u-input v-model="infoList.sex" type="select"
						@click="showSexSheet = true" /></u-form-item>
				<u-action-sheet :list="sexSheetList" v-model="showSexSheet"
					@click="actionSheetCallback"></u-action-sheet>
			</view>

			<u-form-item label="工号" prop="gonghao"><u-input v-model="infoList.gonghao" type="number" /></u-form-item>
			<u-form-item label="工位"><u-input v-model="infoList.gongwei" /></u-form-item>
			<!-- <u-form-item label="工位"></u-form-item> -->
			<!-- 分割条 -->
			<u-line color="blue" border-style="dashed" />
			<!-- 技能 -->
			<view v-for="(item, index) in infoList.skill" :key="index">
				<u-form-item :label="item.head">
					<view class="rateAndtext">
						<view>当前技能等级：{{item.point}}/4</view><u-rate :count="4" v-model="item.point" class="starpoint"
							:colors="starColors"></u-rate>

					</view>

					<view class="calendarAndbutton">
						<view>等级认证日期：{{item.time}}</view>
						<u-calendar v-model="item.calendarShow" mode="date"
							@change="changeDate(index,$event)"></u-calendar>
						<u-button @click="open(index)" type="primary" shape="circle" size="mini">选择日期</u-button>
					</view>
				</u-form-item>
			</view>



		</u-form>


		<!-- 提交按钮 -->
		<u-gap height="80" bg-color="#ffffff"></u-gap>
		<u-toast ref="uToast" />
		<u-button @click="submit" type="warning">提交修改</u-button>
		








	</view>
</template>

<script>
	export default {
		data() {
			return {



				//calendarShow:false,

				starColors: ['#ffc454', '#ff8408', '#ff4c05'],

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

				showSexSheet: false,

				sexSheetList: [{
						text: 'man'
					},
					{
						text: 'woman'
					},

				],

				// 资料卡片信息list
				infoList: {
					src: 'https://tse3-mm.cn.bing.net/th/id/OIP-C.BQLPis_OmX8ELLZDQNjiaQAAAA?rs=1&pid=ImgDetMain',
					sex: "man",
					name: "张三",
					gonghao: "22400111",
					gongwei: "上胶工段43R",
					skillnum: "3",
					skill: [{
							head: '技能1',
							point: 2,
							time: '2024-6-30',
							calendarShow: false,
						},
						{
							head: '技能2',
							point: 3,
							time: '2024-7-30',
							calendarShow: false,
						},
					],

				},

				rules: {
					name: [{
							required: true,
							message: '请输入姓名',
							// 可以单个或者同时写两个触发验证方式 
							trigger: ['change', 'blur'],
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								// 上面有说，返回true表示校验通过，返回false表示不通过
								// this.$u.test.mobile()就是返回true或者false的
								return this.$u.test.chinese(value);
							},
							message: '姓名格式不正确',
							// 触发器可以同时用blur和change
							trigger: ['change', 'blur'],
						}
					],
					gonghao: [{
							pattern: /^[0-9]+$/g,
							// 正则检验前先将值转为字符串
							transform(value) {
								return String(value);
							},
							trigger: ['blur', 'change'],
							message: '工号格式错误'
						},
						{
							required: true,
							len: 8,
							message: '请输入正确的工号',
							// blur和change事件触发检验
							trigger: ['blur', 'change'],
						},



					],


				},




			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},


		methods: {

			showToast() {
				this.$refs.uToast.show({
					title: '修改成功',
					type: 'success',
					back: true,
					// url: '/pages/user/index'
				})
			},

			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						console.log('验证通过');
					} else {
						console.log('验证失败');
					}
				});
				this.showToast();
			},

			open(index) {
				this.infoList.skill[index].calendarShow = true;
				console.log(index);
			},

			changeDate(index, e) {
				this.infoList.skill[index].time = e.result;
				console.log(e);
				console.log(this.infoList);
			},


			actionSheetCallback(index) {
				this.infoList.sex = this.sexSheetList[index].text;
				console.log(this.infoList.sex);
			}

		}
	}
</script>

<style>
	.starpoint {
		padding: 10rpx 20rpx;
		/* margin-right: 70rpx; */
	}

	.rateAndtext {

		display: flex;
		/* 使用Flexbox布局 */
		justify-content: space-between;
		/* 使文字和按钮两端对齐 */
		align-items: center;
		margin-right: 50rpx;
	}

	.calendarAndbutton {

		display: flex;
		/* 使用Flexbox布局 */
		justify-content: space-between;
		/* 使文字和按钮两端对齐 */
		align-items: center;
		margin-right: 50rpx;

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