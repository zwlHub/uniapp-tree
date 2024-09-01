<template>
	<view>
		<xiaolu-tree  :checkList="checkList" 
		:onlyChild="true"  v-if="tree.length>0"  :options="prop" 
		@sendValue="confirm" @toChildren="toChildren"  :isCheck="true" :treeNone="tree"></xiaolu-tree>
	</view>
</template>

<script>
	import { ref } from 'vue';
	import XiaoluTree from '../../components/xiaolu-tree-vue3/tree.vue';
	import { treeNode } from './data.js'
	
	
	const treeData = ref([
		{
			id: 'a-1',
			name: '济南市',
			user: false,
			role: ''
		},
		{
			id: 'a-2',
			name: '青岛市',
			user: false,
			role: ''
		}
	])

	export default {
		components: {
			XiaoluTree
		},
		data() {
			return {
				tree: [],
				checkList: [],
				backList: this.checkList,
				prop:{},
				max: 5,	
			}
		},
		 onLoad(option) {
			 let { prop,arr } = option
			 // #ifdef H5
			let checkList = JSON.parse(decodeURIComponent(arr));
			// #endif
			// #ifdef MP-QQ||MP-WEIXIN||APP-NVUE||APP-PLUS
			let checkList = JSON.parse(arr);
			// #endif
			this.prop = JSON.parse(prop)
			this.checkList = checkList;
			this.tree=treeData.value;//树形数据赋值
		},
		methods: {
			//获取选中的值
			confirm(val,back) {
				console.log("选中项",val)
				if(back){
					this.backConfirm(val)
					return
				}
				this.backList = val;
			},
			toChildren(val) {
				console.log('下一级',val)
				//模拟请求
				setTimeout(() => {
					let response = []
					if(val == 'a-1' ) {
						response = [
							{
								id: 'a-1-1',
								name: '历下区',
								user: false,
								role: ''
							},
							{
								id: 'a-1-2',
								name: '济南用户1',
								user: true,
								role: ''
							},
							{
								id: 'a-1-3',
								name: '济南用户2',
								user: true,
								role: ''
							}
						]
					} else {
						response = [
							{
								id: 'a-2-1',
								name: '黄岛区',
								user: false,
								role: ''
							},
							{
								id: 'a-2-2',
								name: '青岛用户1',
								user: true,
								role: ''
							},
							{
								id: 'a-1-3',
								name: '青岛用户2',
								user: true,
								role: ''
							}
						]
					}
					treeData.value.forEach((item,index) => {
						if( item.id === val ) {
							item.children = response
						}
					})
					this.tree=treeData.value;//树形数据赋值
					uni.$emit('requestEnd',treeData.value)
				},2000)
			},
			// 返回上一页传参
			 backConfirm(val) {
				
				uni.$emit('selectSuccess',{list:val})
				uni.navigateBack();
			}
		}
	}
</script>

<style lang="scss" scoped>
	.box_sizing {
		-webkit-box-sizing: border-box;
		-moz-box-sizing: border-box;
		box-sizing: border-box;
	}

	.btn {
		position: fixed;
		bottom: 0;
		padding: 10px;
		background-color: #fff;
		width: 100%;

		.sureBtn {
			background-color: #0095F2;
			color: #fff;
		}
	}
	
</style>
