<template>
	<div style="width: 100%;height: 100%">
		<div style="display: flex;justify-content: center;">
			<el-button type="primary" @click="addOffV = true" style="margin-left: 10px;" size="small">
				新增登记<i class="el-icon-upload el-icon--right"></i>
			</el-button>
		</div>
		<div style="margin-top: 20px;display:flex;justify-content: center">
			<el-table :data="offdata" border style="width: 1200px">
				<el-table-column type="index" :index="indexMethod" width="49" align="center">
				</el-table-column>
				<el-table-column prop="jdOldpeople.oldpeopleName" label="老人姓名" width="143.75" align="center">
				</el-table-column>
				<el-table-column prop="offTime" label="退住时间" width="143.75" align="center">
				</el-table-column>
				<el-table-column prop="offReason" label="退住原因" width="143.75" align="center">
				</el-table-column>
				<el-table-column prop="serviceNurse.nname" label="登记人" width="143.75" align="center"
					style="font-size: 10px;" show-overflow-tooltip>
				</el-table-column>
				<el-table-column prop="offRtime" label="登记时间" width="143.75" align="center">
				</el-table-column>
				<el-table-column prop="offoperator" label="经办人" width="143.75" align="center">
				</el-table-column>
				<el-table-column prop="offoptime" label="经办时间" width="143.75" align="center">
				</el-table-column>
				<el-table-column prop="offNote" label="备注" width="143.75" align="center" show-overflow-tooltip>
				</el-table-column>
			</el-table>
		</div>
		<div style="margin-top:20px ;display:flex;justify-content: center;position: absolute;width: 100%;">
			<el-pagination @size-change="handleSizeChange1" @current-change="handleCurrentChange1"
				:current-page="pageInfo1.currentPage" :page-sizes="[2, 3, 6]" :page-size="pageInfo1.pagesize"
				layout="total, sizes, prev, pager, next, jumper" :total="pageInfo1.total">
			</el-pagination>
		</div>
	</div>

	<el-dialog title="退住登记" v-model="addOffV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="add">
			<el-form ref="form" :model="oxform" label-width="150px" size="mini" label-position="right">
				<div class="add_content">
					<div class="add_from">
						<el-form-item label="退住时间">
							<input type="date" v-model="oxform.offTime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记人">
							<el-select v-model="oxform.offRegistrant" placeholder="请选择">
								<el-option :key="item.id" :label="item.nname" :value="item.id" v-for="item in ndata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记时间">
							<input type="date" v-model="oxform.offRtimee" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="经办人">
							<el-select v-model="oxform.offoperator" placeholder="请选择">
								<el-option :key="item.id" :label="item.nname" :value="item.id" v-for="item in ndata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="经办时间">
							<input type="date" v-model="oxform.offoptime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="退住原因">
							<el-input v-model="oxform.offReason" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人姓名">
							<el-select v-model="oxform.offOrdid" placeholder="请选择">
								<el-option :key="item.oldpeopleId" :label="item.oldpeopleName" :value="item.oldpeopleId"
									v-for="item in olddata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="oxform.offNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">取 消</el-button>
				<el-button type="primary" @click="addoff" size="small">确 定</el-button>
			</span>
		</template>
	</el-dialog>
</template>

<script>
	import {
		defineComponent,
		ref
	} from 'vue'
	export default defineComponent({
		data() {
			return {
				addname: '',
				input3: ref(''),
				sizeForm: {
					name: '',
					region: '',
					date1: '',
					date2: '',
					delivery: false,
					type: [],
					resource: '',
					desc: ''
				},
				cform: {
					consultingId: '',
					consultingName: '',
					consultingTime: '',
					consultingWay: '',
					consultingContent: '',
					consultingType: '',
					consultingPhone: '',
					consultingRelation: '',
					consultingOldname: '',
					consultingOldidcard: '',
					consultingOldsex: '',
					consultingOldage: '',
					consultingSelfcare: '',
					consultingOrdaddress: '',
					consultingService: '',
					consultingAddname: '',
					consultingRegistrant: '',
					consultingRtime: '',
					consultingNote: ''
				},
				oxform: {
					offid: '',
					offBackid: '',
					offAddname: '',
					offAddtime: '',
					offRegistrant: '',
					offRtime: '',
					offOrdid: '',
					offReason: '',
					offNote: '',
					offTime: '',
					offoperator: '',
					offoptime: ''
				},
				pageInfo1: {
					currentPage: 1,
					pagesize: 2,
					total: 0
				},
				addOffV: false,
				ndata: [],
				offdata: [],
				olddata: []
			};
		},
		methods: {
			close() {
				this.addOffV = false
				for (var key in this.oxform) {
					delete this.oxform[key]
				}
			},
			indexMethod(index) {
				return index + 1;
			},
			handleSizeChange1(pagesize) {
				var _this = this
				this.pageInfo1.pagesize = pagesize
				this.axios.get("http://localhost:8188/selectAllOff", {
						params: _this.pageInfo1
					})
					.then(function(response) {
						console.log(response.data)
						_this.offdata = response.data.data.list
					}).catch(function(error) {
						console.log(error)
					})
			},
			handleCurrentChange1(currentPage) {
				var _this = this
				this.pageInfo1.currentPage = currentPage
				this.axios.get("http://localhost:8188/selectAllOff", {
						params: _this.pageInfo1
					})
					.then(function(response) {
						_this.offdata = response.data.data.list
					}).catch(function(error) {
						console.log(error)
					})
			},
			addoff() {
				var _this = this
				this.addname = this.$store.state.userInfo.userName
				this.axios.post("http://localhost:8188/insertOff/" + this.addname, this.oxform)
					.then(function(response) {
						console.log(response)
						if (response.data.code == 200) {
							_this.$message({
								type: 'success',
								message: '添加成功'
							})
							_this.axios.get("http://localhost:8188/selectAllOff", {
								params: _this.pageInfo1
							}).then(function(response) {
								console.log(response)
								_this.offdata = response.data.data.list
								_this.pageInfo1.total = response.data.data.total
							}).catch(function(error) {
								console.log(error)
							})
						} else {
							_this.$message({
								type: 'info',
								message: '添加失败'
							});
						}
						_this.addOffV = false
						for (var key in _this.oxform) {
							delete _this.oxform[key]
						}
					}).catch(function(error) {
						console.log(error)
						_this.$message({
							type: 'info',
							message: '添加失败'
						});
						for (var key in _this.oxform) {
							delete _this.oxform[key]
						}
					})
			}
		},
		created() {
			const _this = this
			this.axios.get("http://localhost:8188/selectAllOff", {
					params: this.pageInfo1
				})
				.then(function(response) {
					console.log(response)
					_this.offdata = response.data.data.list
					_this.pageInfo1.total = response.data.data.total
				}).catch(function(error) {
					console.log(error)
				})
			this.axios.get("http://localhost:8188/selectAllNurse")
				.then(function(response) {
					console.log(response)
					_this.ndata = response.data.data
				}).catch(function(error) {
					console.log(error)
				})
			this.axios.get("http://localhost:8188/selestOldpeople")
				.then(function(response) {
					console.log(response)
					_this.olddata = response.data.data
				}).catch(function(error) {
					console.log(error)
				})
		}
	});
</script>

<style>
	.add {
		width: 1150px;
		height: 450px;
		margin-top: 20px;
		margin-left: 15px;
		background: white;
		overflow-x: hidden;
		overflow-y: scroll;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	.add::-webkit-scrollbar {
		width: 10px;
	}

	.add::-webkit-scrollbar-thumb {
		border-radius: 5px;
		background: black;
		border: white 3px solid;
	}

	.add::-webkit-scrollbar-track {
		background: white;
	}

	.add_content {
		width: 1150px;
		height: 350px;
		background-color: white;
		display: flex;
		flex-wrap: wrap;
		justify-content: left;
	}

	.add_from {
		margin-left: 10px;
		margin-right: 10px;
		width: 500px;
		height: 50px;
		background-color: white;
	}
</style>
