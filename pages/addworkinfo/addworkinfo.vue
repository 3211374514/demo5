<template>
	<view class="addskillinfo">

		<!-- 修改信息表单 -->
		<u-form :model="addworkinfoList" ref="uForm">
			<u-form-item label="工段" prop="gongduan"><u-input v-model="addworkinfoList.gongduan" /></u-form-item>
			<u-form-item label="工位" prop="gongwei"><u-input v-model="addworkinfoList.gongwei" /></u-form-item>


			<u-form-item prop="time">
				<view class="calendarAndbutton">
					<view>日期：{{addworkinfoList.time}}</view>
					<u-calendar v-model="addworkinfoList.calendarShow" mode="date" @change="changeDate"></u-calendar>
					<u-button @click="open" type="primary" shape="circle" size="mini">选择日期</u-button>
				</view>

			</u-form-item>

			<!-- <u-form-item label="工位"></u-form-item> -->




		</u-form>

		<!-- 提交按钮 -->
		<u-gap height="80" bg-color="#ffffff"></u-gap>
		<u-button @click="submit" type="warning">添加工作经历</u-button>

	</view>
</template>

<script>
	export default {
		data() {
			return {




				addworkinfoList: {
					gongduan: "涂装C线上胶工段",
					gongwei: "CS44R",
					time: "2024.08.19",
					calendarShow: false,

				},

				rules: {
					gongduan: [{
							required: true,
							message: '请输入工段名称',
							// 可以单个或者同时写两个触发验证方式 
							trigger: ['change', 'blur'],
						}

					],
					gongwei: [{
						required: true,
						// min: 1,
						message: '请输入工位名称',
						// blur和change事件触发检验
						trigger: ['blur', 'change'],
					}, ],
					time: [{
						required: true,
						message: '请选择日期',
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
				this.addworkinfoList.calendarShow = true;

			},

			changeDate(e) {
				this.addworkinfoList.time = e.result;
				console.log(e);
				console.log(this.addworkinfoList);
			},

		}
	}
</script>

<style>
	
</style>