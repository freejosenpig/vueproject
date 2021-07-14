<template>
	<div style="width: 100%;height: 100%">
		<div style="display: flex;justify-content: center;">
			<el-button type="primary" @click="addConV = true" style="margin-left: 10px;" size="small">
				新增登记<i class="el-icon-upload el-icon--right"></i>
			</el-button>
		</div>
		<div style="margin-top: 20px;display:flex;justify-content: center">
			<el-table :data="condata" border style="width: 1200px">
				<el-table-column type="index" :index="indexMethod" width="49" align="center">
				</el-table-column>
				<el-table-column prop="consultingName" label="咨询人" width="125" align="center">
				</el-table-column>
				<el-table-column prop="consultingTime" label="咨询时间" width="125" align="center">
				</el-table-column>
				<el-table-column prop="consultingWay" label="咨询方式" width="125" align="center">
				</el-table-column>
				<el-table-column prop="consultingContent" label="咨询内容" width="125" align="center"
					style="font-size: 10px;" show-overflow-tooltip>
				</el-table-column>
				<el-table-column prop="consultingType" label="咨询类型" width="125" align="center">
				</el-table-column>
				<el-table-column prop="consultingPhone" label="联系电话" width="125" align="center">
				</el-table-column>
				<el-table-column prop="serviceNurse2.nname" label="接待人" width="125" align="center">
				</el-table-column>
				<el-table-column prop="serviceNurse.nname" label="登记人" width="125" align="center">
				</el-table-column>
				<el-table-column label="操作" width="150" align="center">
					<template #default="scope">
						<el-button @click="selectcon(scope.row)" type="text" size="small">查看</el-button>
						<el-button @click="updateconshow(scope.row)" type="text" size="small">编辑</el-button>
						<el-button @click="deletecon(scope.row)" type="text" size="small">删除</el-button>
					</template>
				</el-table-column>
			</el-table>
		</div>
		<div style="margin-top:20px ;display:flex;justify-content: center;position: absolute;width: 100%;">
			<el-pagination @size-change="handleSizeChange1" @current-change="handleCurrentChange1"
				:current-page="pageInfo1.a" :page-sizes="[2, 3, 6]" :page-size="pageInfo1.b"
				layout="total, sizes, prev, pager, next, jumper" :total="pageInfo1.total">
			</el-pagination>
		</div>
	</div>

	<el-dialog title="咨询登记" v-model="addConV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="add">
			<el-form ref="form" :model="cform" label-width="150px" size="mini" label-position="right">
				<div class="add_content">
					<div class="add_from">
						<el-form-item label="咨询人姓名">
							<el-input v-model="cform.consultingName" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询时间">
							<input type="date" v-model="cform.consultingTime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询方式">
							<el-select v-model="cform.consultingWay" placeholder="请选择">
								<el-option label="电话" value="电话"></el-option>
								<el-option label="微信" value="微信"></el-option>
								<el-option label="短信" value="短信"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询类型">
							<el-select v-model="cform.consultingType" placeholder="请选择">
								<el-option label="入住咨询" value="入住咨询"></el-option>
								<el-option label="费用咨询" value="费用咨询"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="联系电话">
							<el-input v-model="cform.consultingPhone" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="与老人关系">
							<el-select v-model="cform.consultingRelation" placeholder="请选择">
								<el-option label="本人" value="本人"></el-option>
								<el-option label="父子" value="父子"></el-option>
								<el-option label="父女" value="父女"></el-option>
								<el-option label="母子" value="母子"></el-option>
								<el-option label="母女" value="母女"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人姓名">
							<el-input v-model="cform.consultingOldname" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人身份证">
							<el-input v-model="cform.consultingOldidcard" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人性别">
							<el-select v-model="cform.consultingOldsex" placeholder="请选择">
								<el-option label="男" value="男"></el-option>
								<el-option label="女" value="女"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人年龄">
							<el-input v-model="cform.consultingOldage" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人自理情况">
							<el-select v-model="cform.consultingSelfcare" placeholder="请选择">
								<el-option label="自理" value="自理"></el-option>
								<el-option label="介助" value="介助"></el-option>
								<el-option label="介护" value="介护"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人家庭住址">
							<el-input v-model="cform.consultingOrdaddress" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="接待人">
							<el-select v-model="cform.consultingService" placeholder="请选择">
								<el-option :key="item.id" :label="item.nname" :value="item.id" v-for="item in ndata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记人">
							<el-select v-model="cform.consultingRegistrant" placeholder="请选择">
								<el-option :key="item.id" :label="item.nname" :value="item.id" v-for="item in ndata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记时间">
							<input type="date" v-model="cform.consultingRtime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询内容">
							<el-input v-model="cform.consultingContent" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="cform.consultingNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">取 消</el-button>
				<el-button type="primary" @click="addcon" size="small">确 定</el-button>
			</span>
		</template>
	</el-dialog>

	<el-dialog title="编辑信息" v-model="updateConV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="add">
			<el-form ref="form" :model="cform" label-width="150px" size="mini" label-position="right">
				<div class="add_content">
					<div class="add_from">
						<el-form-item label="咨询人姓名">
							<el-input v-model="cform.consultingName" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询时间">
							<input type="date" v-model="cform.consultingTime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询方式">
							<el-select v-model="cform.consultingWay" placeholder="请选择">
								<el-option label="电话" value="电话"></el-option>
								<el-option label="微信" value="微信"></el-option>
								<el-option label="短信" value="短信"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询类型">
							<el-select v-model="cform.consultingType" placeholder="请选择">
								<el-option label="入住咨询" value="入住咨询"></el-option>
								<el-option label="费用咨询" value="费用咨询"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="联系电话">
							<el-input v-model="cform.consultingPhone" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="与老人关系">
							<el-select v-model="cform.consultingRelation" placeholder="请选择">
								<el-option label="本人" value="本人"></el-option>
								<el-option label="父子" value="父子"></el-option>
								<el-option label="父女" value="父女"></el-option>
								<el-option label="母子" value="母子"></el-option>
								<el-option label="母女" value="母女"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人姓名">
							<el-input v-model="cform.consultingOldname" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人身份证">
							<el-input v-model="cform.consultingOldidcard" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人性别">
							<el-select v-model="cform.consultingOldsex" placeholder="请选择">
								<el-option label="男" value="男"></el-option>
								<el-option label="女" value="女"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人年龄">
							<el-input v-model="cform.consultingOldage" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人自理情况">
							<el-select v-model="cform.consultingSelfcare" placeholder="请选择">
								<el-option label="自理" value="自理"></el-option>
								<el-option label="介助" value="介助"></el-option>
								<el-option label="介护" value="介护"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人家庭住址">
							<el-input v-model="cform.consultingOrdaddress" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="接待人">
							<el-select v-model="cform.consultingService" placeholder="请选择">
								<el-option :key="item.id" :label="item.nname" :value="item.id" v-for="item in ndata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记人">
							<el-select v-model="cform.consultingRegistrant" placeholder="请选择">
								<el-option :key="item.id" :label="item.nname" :value="item.id" v-for="item in ndata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记时间">
							<input type="date" v-model="cform.consultingRtime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="咨询内容">
							<el-input v-model="cform.consultingContent" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="cform.consultingNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">取 消</el-button>
				<el-button type="primary" @click="updatecon" size="small">确 定</el-button>
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
				pageInfo1: {
					a: 1,
					b: 2,
					total: 0
				},
				updateConV: false,
				addConV: false,
				selectConV: false,
				condata: [],
				ndata: []
			};
		},
		methods: {
			close() {
				this.addConV = false
				this.selectConV = false
				this.updateConV = false
				for (var key in this.cform) {
					delete this.cform[key]
				}
			},
			indexMethod(index) {
				return index + 1;
			},
			handleSizeChange1(b) {
				var _this = this
				this.pageInfo1.b = b
				this.axios.get("http://localhost:8188/selectAllConsulting", {
						params: _this.pageInfo1
					})
					.then(function(response) {
						console.log(response.data)
						_this.condata = response.data.data.list
					}).catch(function(error) {
						console.log(error)
					})
			},
			handleCurrentChange1(a) {
				var _this = this
				this.pageInfo1.a = a
				console.log(this.pageInfo1.a)
				this.axios.get("http://localhost:8188/selectAllConsulting", {
						params: _this.pageInfo1
					})
					.then(function(response) {
						_this.condata = response.data.data.list
					}).catch(function(error) {
						console.log(error)
					})
			},
			selectcon(row) {
				this.cform.consultingName = row.consultingName
				this.cform.consultingTime = row.consultingTime
				this.cform.consultingWay = row.consultingWay
				this.cform.consultingContent = row.consultingContent
				this.cform.consultingType = row.consultingType
				this.cform.consultingPhone = row.consultingPhone
				this.cform.consultingRelation = row.consultingRelation
				this.cform.consultingOldname = row.consultingOldname
				this.cform.consultingOldidcard = row.consultingOldidcard
				this.cform.consultingOldsex = row.consultingOldsex
				this.cform.consultingOldage = row.consultingOldage
				this.cform.consultingSelfcare = row.consultingSelfcare
				this.cform.consultingOrdaddress = row.consultingOrdaddress
				this.cform.consultingService = row.consultingService
				this.cform.consultingRegistrant = row.consultingRegistrant
				this.cform.consultingRtime = row.consultingRtime
				this.cform.consultingNote = row.consultingNote
				this.selectConV = true
			},
			updateconshow(row) {
				this.cform.consultingId = row.consultingId
				this.cform.consultingName = row.consultingName
				this.cform.consultingTime = row.consultingTime
				this.cform.consultingWay = row.consultingWay
				this.cform.consultingContent = row.consultingContent
				this.cform.consultingType = row.consultingType
				this.cform.consultingPhone = row.consultingPhone
				this.cform.consultingRelation = row.consultingRelation
				this.cform.consultingOldname = row.consultingOldname
				this.cform.consultingOldidcard = row.consultingOldidcard
				this.cform.consultingOldsex = row.consultingOldsex
				this.cform.consultingOldage = row.consultingOldage
				this.cform.consultingSelfcare = row.consultingSelfcare
				this.cform.consultingOrdaddress = row.consultingOrdaddress
				this.cform.consultingService = row.consultingService
				this.cform.consultingRegistrant = row.consultingRegistrant
				this.cform.consultingRtime = row.consultingRtime
				this.cform.consultingNote = row.consultingNote
				this.updateConV = true
			},
			addcon() {
				var _this = this
				this.addname = this.$store.state.userInfo.userName
				this.axios.post("http://localhost:8188/insertConsulting/" + this.addname, this.cform)
					.then(function(response) {
						console.log(response)
						if (response.data.code == 200) {
							_this.$message({
								type: 'success',
								message: '添加成功'
							})
							_this.axios.get("http://localhost:8188/selectAllConsulting", {
								params: _this.pageInfo1
							}).then(function(response) {
								console.log(response)
								_this.condata = response.data.data.list
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

						_this.addConV = false
						for (var key in _this.cform) {
							delete _this.cform[key]
						}

					}).catch(function(error) {
						console.log(error)
						_this.$message({
							type: 'info',
							message: '添加失败'
						});
						for (var key in _this.cform) {
							delete _this.cform[key]
						}
					})
			},
			updatecon() {
				var _this = this
				this.axios.put("http://localhost:8188/updateConsulting", this.cform)
					.then(function(response) {
						console.log(response)
						if (response.data.code == 200) {
							_this.$message({
								type: 'success',
								message: '修改成功'
							})
							_this.axios.get("http://localhost:8188/selectAllConsulting", {
								params: _this.pageInfo1
							}).then(function(response) {
								console.log(response)
								_this.condata = response.data.data.list
								_this.pageInfo1.total = response.data.data.total
							}).catch(function(error) {
								console.log(error)
							})
						} else {
							_this.$message({
								type: 'info',
								message: '修改失败'
							});
						}

						_this.updateConV = false
						for (var key in _this.cform) {
							delete _this.cform[key]
						}

					}).catch(function(error) {
						console.log(error)
						_this.$message({
							type: 'info',
							message: '修改失败'
						});
						for (var key in _this.cform) {
							delete _this.cform[key]
						}
					})
			},
			deletecon(row) {
				var _this = this
				this.cform.consultingId = row.consultingId
				this.$confirm('您将删除此信息, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.axios.put("http://localhost:8188/deleteConsulting", this.cform)
						.then(function(response) {
							console.log(response)
							if (response.data.code == 200) {
								_this.axios.get("http://localhost:8188/selectAllConsulting", {
									params: _this.pageInfo1
								}).then(function(response) {
									console.log(response)
									_this.condata = response.data.data.list
									_this.pageInfo1.total = response.data.data.total
								}).catch(function(error) {
									console.log(error)
								})
								_this.$message({
									type: 'success',
									message: '删除成功'
								});
							} else {
								_this.$message({
									type: 'info',
									message: '删除失败'
								});
							}

							for (var key in _this.cform) {
								delete _this.cform[key]
							}
						}).catch(function(error) {
							console.log(error)
							for (var key in _this.cform) {
								delete _this.cform[key]
							}
							_this.$message({
								type: 'info',
								message: '删除失败'
							});
						})
				}).catch(() => {
					for (var key in _this.cform) {
						delete _this.cform[key]
					}
					_this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});
			}
		},
		created() {
			const _this = this
			console.log(this.pageInfo1.a)
			this.axios.get("http://localhost:8188/selectAllConsulting", {
					params: this.pageInfo1
				})
				.then(function(response) {
					console.log(response)
					_this.condata = response.data.data.list
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
		height: 600px;
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
