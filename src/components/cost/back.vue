<template>
	<h1>退住界面管理</h1>
	<div>
		<el-form :inline="true" :model="formInline">
			<el-form-item label="老人姓名">
				<el-input v-model="formInline.oldpeopleName" placeholder="老人姓名" clearable></el-input>
			</el-form-item>
			<!-- <el-form-item label="退住金额">
	  		<el-input v-model="formInline.value1" style="width: 100px;"/>-<el-input v-model="formInline.value2"  style="width: 100px;"/>
	  	  </el-form-item> -->
			<el-form-item>
				<el-button type="primary" @click="selectByname">查询</el-button>
			</el-form-item>
		</el-form>
	</div>

	<el-table :data="tableData" style="width: 100%">
		<el-table-column type="index" label="编号" align="center"> </el-table-column>
		<el-table-column prop="jdOldpeople.oldpeopleName" label="老人姓名" align="center"></el-table-column>
		<el-table-column prop="jdOn.onTime" label="入院时间" align="center"></el-table-column>
		<el-table-column prop="prestoreMoney" label="结算时间" align="center"> </el-table-column>
		<el-table-column prop="istoback" label="结算状态" align="center">
			<template #default="scope">
				<p v-if="scope.row.istoback==0">未结算</p>
				<p v-if="scope.row.istoback==1">已结算</p>
			</template>
		</el-table-column>
		<el-table-column label="操作" align="center">
			<template #default="scope">
				<p v-if="scope.row.istoback==0">
					<el-button size="mini" @click="handleEdit(scope.$index,scope.row)">编辑</el-button>
					<el-button size="mini" type="primary" @click="handleJieSuan(scope.row)">结算</el-button>
				</p>
				<p v-if="scope.row.istoback==1">
					<el-button size="mini" type="primary" circle="" icon="el-icon-check"></el-button>
				</p>
			</template>
		</el-table-column>
	</el-table>
	<!-- 退住结算 -->
	<div style="width:100%;height: 50px;" class="addiv" v-bind:style="styleObject">
		<el-col>
			<el-form :model="jiesuanForm">
				缴费老人:
				<el-input v-model="jiesuanForm.oldpeopleName" style="width: 130px;"></el-input>
				应缴总额:
				<el-input v-model="jiesuanForm.backEntryfeesMoney" style="width: 130px;"></el-input>
				应退总额:
				<el-input v-model="jiesuanForm.backMoney" style="width: 130px;"></el-input>
				<p v-if="jiesuanForm.lastmoney<0">
					实收金额:
					<el-input v-model="jiesuanForm.lastmoney" style="width: 130px;"></el-input>
				</p>
				<p v-if="jiesuanForm.lastmoney>=0">
					实退金额:
					<el-input v-model="jiesuanForm.lastmoney" style="width: 130px;"></el-input>
				</p>
				<el-button @click="addJiesuan" style="width: 100px;margin-left: 20px;" type="primary">确定结算</el-button>
				<el-button @click="addcancel" style="width: 100px;margin-left: 20px;" type="danger">取消结算</el-button>
				<!-- <el-button @click="selectlListBy" style="width: 100px;margin-left: 20px;" type="danger">查看详情</el-button> -->
			</el-form>
		</el-col>
	</div>
</template>

<script>
	import { ElMessage } from 'element-plus'
	export default {
		data() {
			return {
				formInline: {
					oldpeopleName: ""
				},
				tableData: [],
				jiesuanDia: false,
				jiesuanForm: {
					oldpeopleName: "",
					backEntryfeesMoney: "",
					backMoney: "",
					lastmoney: "",
					oldpeopleId:""
				},
				updateAccount1:{
					oldpeopleId:"",
					oldpeopleAccount:""
				},
				totalAccount:"",
				styleObject: {
					display: 'none'
				},
				listForm:{
					oldpeopleId:"",
					listMoeny:"",
					listExplain:"",
					listRemark:""
				},
			}
		},
		methods: {
			// //查看流水详情
			// selectlListBy(){
				
			// },
			//新增一条流水
			insertLCostist(oldpeopleId,lastmoney){
				const _this=this
				this.listForm.oldpeopleId=oldpeopleId
				this.listForm.listMoeny=lastmoney
				this.listForm.listExplain="退住结算"
				this.listForm.listRemark="无"
				this.axios.post("http://localhost:8188/insertLCostist",this.listForm)
				.then(function(response){
					console.log(response)
				}).catch(function(error){
					console.log(error)
				})
			},
			//修改为已结算
			addJiesuan(){
				const _this=this
				console.log(this.jiesuanForm.oldpeopleId+"eee")
				this.axios.put("http://localhost:8188/updateBackByKey",this.jiesuanForm)
				.then(function(response){
					console.log(response)
					_this.updateAccount(_this.jiesuanForm.oldpeopleId,_this.jiesuanForm.lastmoney)
					_this.insertLCostist(_this.jiesuanForm.oldpeopleId,_this.jiesuanForm.lastmoney)
					ElMessage.success({
					   message: '退住结算成功',
					   type: 'success'
					})
					setTimeout(function () {
					  window.location.reload()
					}, 1000)
				}).catch(function(error){
					console.log(error)
				})
			},
			//根据老人编号修改账户余额
			updateAccount(OldId,account){
				const _this=this
				this.updateAccount1.oldpeopleId=OldId
				this.updateAccount1.oldpeopleAccount=account
				console.log(this.updateAccount1.oldpeopleId+"bbbb")
				console.log(this.updateAccount1.oldpeopleAccount+"cccc")
				//根据老人编号查询所有老人信息
				this.axios.get("http://localhost:8188/selectByPrimaryKey/"+this.updateAccount1.oldpeopleId)
				.then(function(response){
					console.log(response)
					_this.totalAccount=response.data.data.oldpeopleAccount
					console.log(_this.totalAccount+"总账户")
					console.log(response.data.data.oldpeopleAccount+"总账户")
					_this.updateAccount1.oldpeopleAccount=_this.totalAccount-_this.jiesuanForm.backMoney
					console.log(_this.updateAccount1.oldpeopleAccount+"剩余金额")
					_this.axios.put("http://localhost:8188/updateAccount",_this.updateAccount1)
					.then(function(response){
						console.log(response)
					}).catch(function(error){
						console.log(error)
					})
				}).catch(function(error){
					console.log(error)
				})
			},
			handleJieSuan(row) {
				const _this = this
				this.jiesuanDia = true
				this.styleObject.display = 'block'
				this.jiesuanForm.oldpeopleName = row.jdOldpeople.oldpeopleName
				this.axios.get("http://localhost:8188/selectBackByoldId/" + row.oldpeopleId)
				.then(function(response) {
					console.log(response)
					_this.jiesuanForm.oldpeopleId=response.data.jdOldpeople.oldpeopleId
					_this.jiesuanForm.backEntryfeesMoney = response.data.backEntryfeesMoney
					//应退金额小于0,最终金额等于
					_this.jiesuanForm.backMoney=response.data.backMoney
					console.log(response.data.backMoney+"pppp")
					if(response.data.backMoney=0){
						_this.jiesuanForm.lastmoney=response.data.backEntryfeesMoney
					}else if(response.data.backMoney<0){
						_this.jiesuanForm.lastmoney = response.data.backMoney - response.data.backEntryfeesMoney
					}
					console.log(_this.jiesuanForm.lastmoney+"last")
				}).catch(function(error) {
					console.log(error)
				})	
			},
			selectByname() {
				const _this = this
				this.axios.get("http://localhost:8188/selectBycontion/" + this.formInline.oldpeopleName)
				.then(function(response) {
					console.log(response)
					_this.tableData = response.data.data
				}).catch(function(error) {
					console.log(error)
				})	
			},
			addcancel() {
				this.styleObject.display = 'none'
			}
		},
		created() {
			const _this = this
			this.axios.get("http://localhost:8188/selectBackAll")
				.then(function(response) {
					console.log(response)
					_this.tableData = response.data.data
				}).catch(function(error) {
					console.log(error)
				})
		}
	}
</script>

<style>
</style>
