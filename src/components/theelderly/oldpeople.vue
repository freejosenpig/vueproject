<template>
	<div style="width: 100%;height: 100%">
		<div style="margin-top: 20px;display:flex;justify-content: center">
			<el-table :data="opdata" border style="width: 1200px">
				<el-table-column type="index" :index="indexMethod" width="49" align="center">
				</el-table-column>
				<el-table-column prop="oldpeopleName" label="姓名" width="125" align="center">
				</el-table-column>
				<el-table-column prop="oldpeopleSex" label="性别" width="125" align="center">
				</el-table-column>
				<el-table-column prop="oldpeopleAge" label="年龄" width="125" align="center">
				</el-table-column>
				<el-table-column prop="oldpeopleIdcard" label="身份证号" width="125" align="center" show-overflow-tooltip>
				</el-table-column>
				<el-table-column prop="oldpeoplePhone" label="联系电话" width="125" align="center" show-overflow-tooltip>
				</el-table-column>
				<el-table-column prop="serviceNurse.nname" label="登记人" width="125" align="center">
				</el-table-column>
				<el-table-column prop="oldpeopleRtime" label="登记时间" width="125" align="center">
				</el-table-column>
				<el-table-column prop="oldpeopleNote" label="备注" width="125" align="center" show-overflow-tooltip>
				</el-table-column>

				<el-table-column label="操作" width="150" align="center">
					<template #default="scope">
						<el-button @click="selectop(scope.row)" type="text" size="small">查看</el-button>
						<el-button @click="updateopshow(scope.row)" type="text" size="small">编辑</el-button>
					</template>
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

	<el-dialog title="老人基本信息" v-model="selectOpV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="add">
			<el-form ref="form" :model="oform" label-width="150px" size="mini" label-position="right">
				<div class="add_content">
					<div class="add_from">
						<el-form-item label="老人姓名">
							<el-input placeholder="请输入内容" v-model="oform.oldpeopleName">
							</el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="身份证号">
							<el-input v-model="oform.oldpeopleIdcard" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="性别">
							<el-input v-model="oform.oldpeopleSex" placeholder="请输入内容"></el-input>
						</el-form-item>

					</div>
					<div class="add_from">
						<el-form-item label="年龄">
							<el-input v-model="oform.oldpeopleAge" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="生日">
							<el-input v-model="oform.oldpeopleBirthday" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="联系电话">
							<el-input v-model="oform.oldpeoplePhone" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="婚姻状况">
							<el-input v-model="oform.oldpeopleMarriage" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="民族">
							<el-input v-model="oform.oldpeopleNational" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="政治面貌">
							<el-input v-model="oform.oldpeoplePolitical" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="退休职业">
							<el-input v-model="oform.oldpeopleJob" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="家庭住址">
							<el-input v-model="oform.oldpeopleAddress" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="爱好">
							<el-input v-model="oform.oldpeopleHobby" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="性格心理">
							<el-input v-model="oform.oldpeopleCharacter" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="饮食特点">
							<el-input v-model="oform.oldpeopleLikefood" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="文化程度">
							<el-input v-model="oform.oldpeopleCulture" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="劳动能力">
							<el-input v-model="oform.oldpeopleAbility" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="医疗保障">
							<el-input v-model="oform.oldpeopleMedical" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="医疗卡号">
							<el-input v-model="oform.oldpeopleMedicalid" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="oform.oldpeopleNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">关 闭</el-button>
			</span>
		</template>
	</el-dialog>

	<el-dialog title="老人基本信息修改" v-model="updateOldV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="add">
			<el-form ref="form" :model="oform" label-width="150px" size="mini" label-position="right">
				<div class="add_content">
					<div class="add_from">
						<el-form-item label="老人姓名">
							<el-input v-model="oform.oldpeopleName" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="身份证号">
							<el-input v-model="oform.oldpeopleIdcard" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="性别">
							<el-select v-model="oform.oldpeopleSex" placeholder="请选择">
								<el-option label="男" value="男"></el-option>
								<el-option label="女" value="女"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="年龄">
							<el-input v-model="oform.oldpeopleAge" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="生日">
							<input type="date" v-model="oform.oldpeopleBirthday" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="联系电话">
							<el-input v-model="oform.oldpeoplePhone" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="婚姻状况">
							<el-select v-model="oform.oldpeopleMarriage" placeholder="请选择">
								<el-option label="已婚" value="已婚"></el-option>
								<el-option label="未婚" value="未婚"></el-option>
								<el-option label="离异" value="离异"></el-option>
								<el-option label="丧偶" value="丧偶"></el-option>
								<el-option label="再婚" value="再婚"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="民族">
							<el-input v-model="oform.oldpeopleNational" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="政治面貌">
							<el-select v-model="oform.oldpeoplePolitical" placeholder="请选择">
								<el-option label="群众" value="群众"></el-option>
								<el-option label="中共党员" value="中共党员"></el-option>
								<el-option label="中共预备党员" value="中共预备党员"></el-option>
								<el-option label="共青团员" value="共青团员"></el-option>
								<el-option label="民革党员" value="民革党员"></el-option>
								<el-option label="民盟盟员" value="民盟盟员"></el-option>
								<el-option label="民建会员" value="民建会员"></el-option>
								<el-option label="民进会员" value="民进会员"></el-option>
								<el-option label="农工党党员" value="农工党党员"></el-option>
								<el-option label="致公党党员" value="致公党党员"></el-option>
								<el-option label="九三学社社员" value="九三学社社员"></el-option>
								<el-option label="台盟盟员" value="台盟盟员"></el-option>
								<el-option label="无党派人士" value="无党派人士"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="退休职业">
							<el-input v-model="oform.oldpeopleJob" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="家庭住址">
							<el-input v-model="oform.oldpeopleAddress" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="爱好">
							<el-input v-model="oform.oldpeopleHobby" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="性格心理">
							<el-input v-model="oform.oldpeopleCharacter" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="饮食特点">
							<el-input v-model="oform.oldpeopleLikefood" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="文化程度">
							<el-input v-model="oform.oldpeopleCulture" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="劳动能力">
							<el-select v-model="oform.oldpeopleAbility" placeholder="请选择">
								<el-option label="较强" value="较强"></el-option>
								<el-option label="一般" value="一般"></el-option>
								<el-option label="较差" value="较差"></el-option>
								<el-option label="极差" value="极差"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="医疗保障">
							<el-input v-model="oform.oldpeopleMedical" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="医疗卡号">
							<el-input v-model="oform.oldpeopleMedicalid" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记人">
							<el-select v-model="oform.oldpeopleRegistrant" placeholder="请选择">
								<el-option :key="item.id" :label="item.nname" :value="item.id" v-for="item in ndata">
								</el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记时间">
							<input type="date" v-model="oform.oldpeopleRtime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="oform.oldpeopleNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">取 消</el-button>
				<el-button @click="updateop" size="small">保 存</el-button>
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
				condition: {
					what: ''
				},
				selectOpV: false,
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
				oform: {
					oldpeopleId: '',
					oldpeopleName: '',
					oldpeopleSex: '',
					oldpeopleIdcard: '',
					oldpeopleBirthday: '',
					oldpeoplePhone: '',
					oldpeopleMarriage: '',
					oldpeopleNational: '',
					oldpeoplePolitical: '',
					oldpeopleJob: '',
					oldpeopleAddress: '',
					oldpeopleHobby: '',
					oldpeopleCharacter: '',
					oldpeopleLikefood: '',
					oldpeopleCulture: '',
					oldpeopleAbility: '',
					oldpeopleMedical: '',
					oldpeopleMedicalid: '',
					oldpeopleNumber: '',
					oldpeopleAddname: '',
					oldpeopleAge: '',
					oldpeopleRegistrant: '',
					oldpeopleRtime: '',
					oldpeopleNote: '',
				},
				pageInfo1: {
					currentPage: 1,
					pagesize: 2,
					total: 0
				},
				addOffV: false,
				ndata: [],
				offdata: [],
				olddata: [],
				opdata: [],
				updateOldV: false
			};
		},
		methods: {
			close() {
				this.updateOldV = false
				this.selectOpV = false
				for (var key in _this.oform) {
					delete _this.oform[key]
				}
			},
			updateopshow(row) {
				this.updateOldV = true
				this.oform.oldpeopleId = row.oldpeopleId
				this.oform.oldpeopleName = row.oldpeopleName
				this.oform.oldpeopleSex = row.oldpeopleSex
				this.oform.oldpeopleIdcard = row.oldpeopleIdcard
				this.oform.oldpeopleBirthday = row.oldpeopleBirthday
				this.oform.oldpeoplePhone = row.oldpeoplePhone
				this.oform.oldpeopleMarriage = row.oldpeopleMarriage
				this.oform.oldpeopleNational = row.oldpeopleNational
				this.oform.oldpeoplePolitical = row.oldpeoplePolitical
				this.oform.oldpeopleJob = row.oldpeopleJob
				this.oform.oldpeopleAddress = row.oldpeopleAddress
				this.oform.oldpeopleHobby = row.oldpeopleHobby
				this.oform.oldpeopleCharacter = row.oldpeopleCharacter
				this.oform.oldpeopleLikefood = row.oldpeopleLikefood
				this.oform.oldpeopleCulture = row.oldpeopleCulture
				this.oform.oldpeopleAbility = row.oldpeopleAbility
				this.oform.oldpeopleMedical = row.oldpeopleMedical
				this.oform.oldpeopleMedicalid = row.oldpeopleMedicalid
				this.oform.oldpeopleNumber = row.oldpeopleNumber
				this.oform.oldpeopleAddname = row.oldpeopleAddname
				this.oform.oldpeopleAge = row.oldpeopleAge
				this.oform.oldpeopleRegistrant = row.oldpeopleRegistrant
				this.oform.oldpeopleRtime = row.oldpeopleRtime
				this.oform.oldpeopleNote = row.oldpeopleNote
			},
			updateop() {
				var _this = this
				this.axios.put("http://localhost:8188/updateOldpeople", this.oform)
					.then(function(response) {
						console.log(response)
						if (response.data.code == 200) {
							_this.$message({
								type: 'success',
								message: '修改成功'
							})
							_this.axios.get("http://localhost:8188/selectAllOldpeople", {
									params: _this.pageInfo1
								})
								.then(function(response) {
									console.log(response)
									_this.opdata = response.data.data.list
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
						_this.updateOldV = false
						for (var key in _this.oform) {
							delete _this.oform[key]
						}
					}).catch(function(error) {
						console.log(error)
						_this.$message({
							type: 'info',
							message: '修改失败'
						});
						for (var key in _this.oform) {
							delete _this.oform[key]
						}
					})
			},
			selectop(row) {
				this.selectOpV = true
				this.oform.oldpeopleName = row.oldpeopleName
				this.oform.oldpeopleSex = row.oldpeopleSex
				this.oform.oldpeopleIdcard = row.oldpeopleIdcard
				this.oform.oldpeopleBirthday = row.oldpeopleBirthday
				this.oform.oldpeoplePhone = row.oldpeoplePhone
				this.oform.oldpeopleMarriage = row.oldpeopleMarriage
				this.oform.oldpeopleNational = row.oldpeopleNational
				this.oform.oldpeoplePolitical = row.oldpeoplePolitical
				this.oform.oldpeopleJob = row.oldpeopleJob
				this.oform.oldpeopleAddress = row.oldpeopleAddress
				this.oform.oldpeopleHobby = row.oldpeopleHobby
				this.oform.oldpeopleCharacter = row.oldpeopleCharacter
				this.oform.oldpeopleLikefood = row.oldpeopleLikefood
				this.oform.oldpeopleCulture = row.oldpeopleCulture
				this.oform.oldpeopleAbility = row.oldpeopleAbility
				this.oform.oldpeopleMedical = row.oldpeopleMedical
				this.oform.oldpeopleMedicalid = row.oldpeopleMedicalid
				this.oform.oldpeopleNumber = row.oldpeopleNumber
				this.oform.oldpeopleAddname = row.oldpeopleAddname
				this.oform.oldpeopleAge = row.oldpeopleAge
				this.oform.oldpeopleRegistrant = row.oldpeopleRegistrant
				this.oform.oldpeopleRtime = row.oldpeopleRtime
				this.oform.oldpeopleNote = row.oldpeopleNote
			},
			indexMethod(index) {
				return index + 1;
			},
			handleSizeChange1(pagesize) {
				var _this = this
				this.pageInfo1.pagesize = pagesize
				this.axios.get("http://localhost:8188/selectAllOldpeople", {
						params: _this.pageInfo1
					})
					.then(function(response) {
						console.log(response.data)
						_this.opdata = response.data.data.list
					}).catch(function(error) {
						console.log(error)
					})
			},
			handleCurrentChange1(currentPage) {
				var _this = this
				this.pageInfo1.currentPage = currentPage
				this.axios.get("http://localhost:8188/selectAllOldpeople", {
						params: _this.pageInfo1
					})
					.then(function(response) {
						_this.opdata = response.data.data.list
					}).catch(function(error) {
						console.log(error)
					})
			}
		},
		created() {
			const _this = this
			this.axios.get("http://localhost:8188/selectAllOldpeople", {
					params: this.pageInfo1
				})
				.then(function(response) {
					console.log(response)
					_this.opdata = response.data.data.list
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
		height: 500px;
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
