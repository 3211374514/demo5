<template>
	<view class="addskillinfo">

		<!-- 修改信息表单 -->
		<u-form :model="addinfoList" ref="uForm">
			<u-form-item label="技能名称" prop="skill"><u-input v-model="addinfoList.skill" /></u-form-item>


			<u-form-item label="熟练度等级">
				<!-- <u-input v-model="addinfoList.point" /> -->
				<u-rate :count="4" :min-count="1" v-model="addinfoList.point" class="starpoint" :colors="starColors"></u-rate>
			</u-form-item>

			<u-form-item prop="time">
				<view class="calendarAndbutton">
					<view>等级认证日期：{{addinfoList.time}}</view>
					<u-calendar v-model="addinfoList.calendarShow" mode="date" @change="changeDate"></u-calendar>
					<u-button @click="open" type="primary" shape="circle" size="mini">选择日期</u-button>
				</view>

			</u-form-item>

			<!-- <u-form-item label="工位"></u-form-item> -->




		</u-form>

		<!-- 提交按钮 -->
		<u-gap height="80" bg-color="#ffffff"></u-gap>
		<u-button @click="submit" type="warning">添加技能</u-button>

	</view>
</template>

<script>
	export default {
		data() {
			return {



				starColors: ['#ffc454', '#ff8408', '#ff4c05'],

				addinfoList: {
					skill: "赏识在于角度的转换",
					point: 1,
					time: "",
					calendarShow: false,

				},

				rules: {
					skill: [{
							required: true,
							message: '请输入技能名称',
							// 可以单个或者同时写两个触发验证方式 
							trigger: ['change', 'blur'],
						}

					],
					point: [{
						required: true,
						// min: 1,
						message: '请选择技能熟练度等级',
						// blur和change事件触发检验
						trigger: ['blur', 'change'],
					}, ],
					time: [{
						required: true,
						message: '请选择技能等级认证时间',
						// blur和change事件触发检验
						trigger: ['blur', 'change'],
					}, ],


				},

			}
		},

		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},


		methods: {

			

			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						console.log('验证通过');
					} else {
						console.log('验证失败');
					}
				});
				console.log(this.addinfoList);
			},

			open() {
				this.addinfoList.calendarShow = true;

			},

			changeDate(e) {
				this.addinfoList.time = e.result;
				console.log(e);
				console.log(this.addinfoList);
			},

		}
	}
</script>

<style>
	.starpoint {
		padding: 10rpx 20rpx;
		/* margin-right: 70rpx; */
	}
</style>