<template>
	<view class="container">
		<!-- 搜索框 -->
		<view class="search-bar">
			<input class="search-input" type="text" v-model="searchQuery" placeholder="搜索" />
		</view>
		<!-- 下拉菜单选择 -->
		<view class="filter-bar">
			<picker mode="selector" :range="groupOptions" @change="selectFilter('group', $event.detail.value)">
				<view class="picker">{{ selectedFilters.group|| '班组' }}</view>
			</picker>

			<picker mode="selector" :range="skillOptions" @change="selectFilter('skill', $event.detail.value)">
				<view class="picker">{{ selectedFilters.skill|| '技能' }}</view>
			</picker>

			<picker mode="selector" :range="statusOptions" @change="selectFilter('status', $event.detail.value)">
				<view class="picker">{{ selectedFilters.status|| '状态' }}</view>
			</picker>
		</view>
		<!-- 筛选后的人员信息 -->
		<view v-for="person in filteredList" :key="person.id" class="person-card">
			<view class="user-info">
				<image class="avatar" src="../../static/touxiang.png"></image>
				<view class="user-details">
					<text class="user-name">{{ person.name }}</text>
					<text class="user-status">状态: {{ person.status }}</text>
					<text class="user-group">所属班组: {{ person.group }}</text>
					<text class="user-time">工作时间: {{ person.workTime }}</text>
				</view>
			</view>
			<view class="user-skill">
				<text>●掌握技能: {{ person.skill }}</text>
				<text>●熟练程度: {{ person.proficiency }}</text>
				<text>●来岗时间: {{ person.joinDate }}</text>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				searchQuery: "",

				// 可选项
				groupOptions: ["班组", "上胶1班", "上胶2班"],
				skillOptions: ["技能", "涂装", "打磨","注胶","套色"],
				statusOptions: ["状态", "在岗", "离岗"],

				// 当前选中项
				selectedFilters: {
					group: "班组",
					skill: "技能",
					status: "状态"
				},

				// 人员信息
				people: [{
						id: 1,
						name: "张三",
						status: "在岗",
						group: "上胶1班",
						workTime: "9:00-17:00",
						skill: "涂装",
						proficiency: "XXXXX",
						joinDate: "XXXXX",
						image: "path_to_image_1"
					},
					{
						id: 2,
						name: "李四",
						status: "离岗",
						group: "上胶2班",
						workTime: "9:00-17:00",
						skill: "打磨",
						proficiency: "XXXXX",
						joinDate: "XXXXX",
						image: "path_to_image_2"
					},
					{
						id: 3,
						name: "赵力",
						status: "离岗",
						group: "上胶1班",
						workTime: "9:00-17:00",
						skill: "注胶",
						proficiency: "XXXXX",
						joinDate: "XXXXX",
						image: "path_to_image_2"
					},
					{
						id: 4,
						name: "王明",
						status: "离岗",
						group: "上胶2班",
						workTime: "9:00-17:00",
						skill: "套色",
						proficiency: "XXXXX",
						joinDate: "XXXXX",
						image: "path_to_image_2"
					},
					// 其他人员信息...
				]
			};
		},
		computed: {
			filteredList() {
				return this.people.filter(person => {
					return (
						(this.selectedFilters.group === "班组" || person.group === this.selectedFilters
							.group) &&
						(this.selectedFilters.skill === "技能" || person.skill === this.selectedFilters
							.skill) &&
						(this.selectedFilters.status === "状态" || person.status === this.selectedFilters
							.status) &&
						(person.name.includes(this.searchQuery) ||
							person.group.includes(this.searchQuery) ||
							person.skill.includes(this.searchQuery) ||
							person.status.includes(this.searchQuery))
					);
				});
			}
		},
		methods: {
			selectFilter(type, index) {
				const options = {
					group: this.groupOptions,
					skill: this.skillOptions,
					status: this.statusOptions
				};
				this.selectedFilters[type] = options[type][index];
			}
		}
	};
</script>

<style scoped>
	.container {
		padding: 10px;
	}

	.search-bar {
		display: flex;
		justify-content: center;
		margin-bottom: 10px;
	}

	.search-input {
		width: 90%;
		padding: 10px;
		border-radius: 6px;
		border: 1px solid #ddd;
	}

	.person-card {
		background-color: #fff;
		margin-bottom: 10px;
		padding: 15px;
		border-radius: 10px;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
	}

	.user-info {
		display: flex;
		margin-bottom: 10px;
	}

	.avatar {
		width: 80px;
		height: 80px;
		border-radius: 50%;
		margin-right: 10px;
	}

	.user-details {
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding-left: 15px;
	}

	.user-name {
		font-size: 18px;
		font-weight: bold;
	}

	.user-status,
	.user-group,
	.user-time {
		font-size: 14px;
		margin-top: 5px;
	}

	.user-skill {
		font-size: 14px;
		line-height: 23px;
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding-left: 100px;
		margin-top: 5px;
	}

	.filter-bar {
		padding-left: 20px;
		padding-right: 20px;
		display: flex;
		justify-content: space-between;
		margin-bottom: 15px;
	}

	.picker {
		padding: 7px;

		box-shadow: 0 0 4px rgba(0, 0, 0, 0.5);
		border-radius: 10px;
		width: 70px;
		margin-right: 10px;
		text-align: center;
	}
</style>