<template>
	<view>

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
		<!-- 个人技能选择 -->
		<view class="skillcheckbox" v-if="curNow==0">
			<u-checkbox-group @change="skillcheckboxGroupChange">
				<u-checkbox @change="skillcheckboxChange" v-model="item.checked" v-for="(item, index) in skillitemList"
					:key="index" :name="item.head" :disabled="false">
					<view class="collapse">
						<u-collapse :accordion="false">
							<u-collapse-item :title="item.head">
								<template v-slot:title>
									<view class="title-container">
										<text>{{item.head}}</text>
										<!-- 进度条 -->
										<u-line-progress active-color="#2979ff" :percent="(item.point/4)*100"
											class="progress" :striped="true" :striped-active="true"></u-line-progress>
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
					</view>
				</u-checkbox>
			</u-checkbox-group>
			<u-button @click="checkedAllSkill">全选</u-button>
			<!-- 提交按钮 -->
			<u-gap height="80" bg-color="#ffffff"></u-gap>
			<u-button @click="submit" type="warning">删除技能</u-button>
			<u-gap height="80" bg-color="#ffffff"></u-gap>
		</view>
		
		<!-- 工作经历选择 -->
		<view class="workcheckbox" v-if="curNow==1">
			<u-checkbox-group @change="workcheckboxGroupChange">
				<u-checkbox @change="workcheckboxChange" v-model="item.checked" v-for="(item, index) in workitemlist"
					:key="index" :name="item.gongwei" :disabled="false">
					<u-cell-item :arrow="false" icon="setting-fill" :title="item.gongduan" :value="item.gongwei"
						:label="item.time"></u-cell-item>
				</u-checkbox>
			</u-checkbox-group>
			<!-- 全选 -->
			<u-button @click="checkedAllWork">全选</u-button>
			<!-- 提交按钮 -->
			<u-gap height="80" bg-color="#ffffff"></u-gap>
			<u-button @click="submit" type="warning">删除工作经历</u-button>
			<u-gap height="80" bg-color="#ffffff"></u-gap>
		</view>



	</view>
</template>

<script>
	export default {
		data() {
			return {
				
				workitemlist: [{
					gongduan: "涂装C线上胶工段",
					gongwei: "CS44R",
					time: "2024.08.19",
					checked: false,
				}, {
					gongduan: "涂装C线打磨工段",
					gongwei: "CS44R",
					time: "2023.06.10",
					checked: false,
				}, {
					gongduan: "涂装C线上胶工段",
					gongwei: "CS44R",
					time: "2024.08.19",
					checked: false,
				}],

				
				starColors: ['#ffc454', '#ff8408', '#ff4c05'],

				curNow: 0,

				list: [

					{
						name: '删除技能'
					},
					{
						name: '删除工作经历'
					},


				],

				checkedSkill: [],
				checkedWork: [],

				skillitemList: [{
					head: "赏识在于角度的转换",
					point: 1,
					time: "只要我们正确择取一个合适的参照物乃至稍降一格去看待他人，值得赏识的东西便会扑面而来",
					checked: false,
					disabled: false,
				}, {
					head: "生活中不是缺少美，而是缺少发现美的眼睛",
					point: 2,
					time: "学会欣赏，实际是一种积极生活的态度，是生活的调味品，会在欣赏中发现生活的美",
					checked: false,
					disabled: false
				}, {
					head: "周围一些不起眼的人、事、物，或许都隐藏着不同凡响的智慧",
					point: 4,
					time: "但是据说雕刻大卫像所用的这块大理石，曾被多位雕刻家批评得一无是处，有些人认为这块大理石采凿得不好，有些人嫌它的纹路不够美",
					checked: false,
					disabled: false
				}],

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
				
				console.log("checkedSkill:"+this.checkedSkill);
				console.log("checkedWork:"+this.checkedWork);
			},

			sectionChange(index) {
				this.curNow = index;
				console.log(this.curNow);
			},

			skillcheckboxChange(e) {
				// console.log(e);
			},
			// 选中任一checkbox时，由checkbox-group触发
			skillcheckboxGroupChange(e) {
				console.log(e);
				this.checkedSkill = e;
				
				console.log("checkedSkill:"+this.checkedSkill);
			},
			workcheckboxChange(e) {
				// console.log(e);
			},
			// 选中任一checkbox时，由checkbox-group触发
			workcheckboxGroupChange(e) {
				console.log(e);
				this.checkedWork = e;
				
				console.log("checkedSkill:"+this.checkedWork);
			},
			// 全选
			checkedAllSkill() {
				this.skillitemList.map(val => {
					val.checked = true;
				})
			},
			checkedAllWork() {
				this.workitemlist.map(val => {
					val.checked = true;
				})
			}

		}
	}
</script>

<style>
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