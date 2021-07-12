<template>
	<div style="width: 100%;height: 100%">
		<div style="display: flex;justify-content: center;">
			<el-button type="primary" @click="addOldV = true" style="margin-left: 10px;" size="small">
				新增登记<i class="el-icon-upload el-icon--right"></i>
			</el-button>
		</div>
		<div style="margin-top: 20px;display:flex;justify-content: center">
			<el-table :data="condata" border style="width: 1200px">
				<el-table-column type="index" :index="indexMethod" width="49" align="center">
				</el-table-column>
				<el-table-column prop="consultingName" label="咨询人" width="120" align="center">
				</el-table-column>
				<el-table-column prop="consultingTime" label="咨询时间" width="120" align="center">
				</el-table-column>
				<el-table-column prop="consultingWay" label="咨询方式" width="100" align="center">
				</el-table-column>
				<el-table-column prop="consultingContent" label="咨询内容" width="180" align="center"
					style="font-size: 10px;" show-overflow-tooltip>
				</el-table-column>
				<el-table-column prop="consultingType" label="咨询类型" width="100" align="center">
				</el-table-column>
				<el-table-column prop="consultingPhone" label="联系电话" width="150" align="center">
				</el-table-column>
				<el-table-column prop="serviceNurse2.nname" label="接待人" width="100" align="center">
				</el-table-column>
				<el-table-column prop="serviceNurse.nname" label="登记人" width="100" align="center">
				</el-table-column>
				<el-table-column label="操作" width="180" align="center">
					<template #default="scope">
						<el-button @click="selectcon(scope.row)" type="text" size="small">查看</el-button>
						<el-button @click="updateconshow(scope.row)" type="text" size="small">编辑</el-button>
						<el-button @click="deletecon(scope.row)" type="text" size="small">删除</el-button>
					</template>
				</el-table-column>
			</el-table>
		</div>
		<div style="margin-top:20px ;display:flex;justify-content: center;position: absolute;width: 100%;">
			<el-pagination background @size-change="handleSizeChange1" @current-change="handleCurrentChange1"
				:current-page="pageInfo1.currentPage" :page-sizes="[2, 3, 6]" :page-size="pageInfo1.pagesize"
				layout="total, sizes, prev, pager, next, jumper" :total="pageInfo1.total">
			</el-pagination>
		</div>
	</div>
	<el-dialog title="老人基本信息登记" v-model="addOldV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="prompt" style="color: limegreen;">
			提示：老人基本信息登记 > 家属信息登记 > 健康评估登记 > 选择床位 > 入住缴费
		</div>
		<div class="add">
			<el-form ref="form" :model="oform" label-width="150px" size="mini" label-position="right">
				<div class="add_content">
					<div class="add_from">
						<el-form-item label="老人姓名">
							<el-input placeholder="请输入内容" v-model="oform.oldpeopleName" class="input-with-select">
								<template #prepend>
									<el-tooltip content="选择咨询登记过的老人" placement="top">
										<el-select v-model="condition.what" placeholder="请选择">
											<el-option v-for="item in condata" :key="item.consultingId"
												:label="item.consultingOldname" :value="item.consultingId"></el-option>
										</el-select>
									</el-tooltip>
								</template>
								<template #append>
									<el-tooltip content="查询信息" placement="top">
										<el-button icon="el-icon-search" @click="selectconByid"></el-button>
									</el-tooltip>
								</template>
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
						<el-form-item label="登记人">
							<el-select v-model="oform.oldpeopleRegistrant" placeholder="请选择">
								<el-option label="2" value="2"></el-option>
								<el-option label="3" value="3"></el-option>
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
				<el-button @click="save1" size="small">保 存</el-button>
				<el-button @click="dsave1" size="small">清除保存</el-button>
				<el-button @click="next1" size="small">下一步</el-button>
			</span>
		</template>
	</el-dialog>

	<el-dialog title="家属信息登记" v-model="addFamilyV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="prompt" style="color: limegreen;">
			提示：老人基本信息登记 > 家属信息登记 > 健康评估登记 > 选择床位 > 入住缴费
		</div>
		<div class="add2">
			<el-form ref="form" :model="fform" label-width="150px" size="mini" label-position="right">
				<div class="add_content2">
					<div class="add_from">
						<el-form-item label="家属姓名">
							<el-input v-model="fform.familyName" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="家属联系电话">
							<el-input v-model="fform.familyPhone" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="家属联系地址">
							<el-input v-model="fform.familyAddress" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="家属与老人关系">
							<el-select v-model="fform.familyRelation" placeholder="请选择">
								<el-option label="父子" value="父子"></el-option>
								<el-option label="父女" value="父女"></el-option>
								<el-option label="母子" value="母子"></el-option>
								<el-option label="母女" value="母女"></el-option>
								<el-option label="其他" value="其他"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记人">
							<el-select v-model="fform.familyRegistrant" placeholder="请选择">
								<el-option label="2" value="2"></el-option>
								<el-option label="3" value="3"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记时间">
							<input type="date" v-model="fform.familyRtime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="fform.familyNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">取 消</el-button>
				<el-button @click="save2" size="small">保 存</el-button>
				<el-button @click="dsave2" size="small">清除保存</el-button>
				<el-button @click="last2" size="small">上一步</el-button>
				<el-button @click="next2" size="small">下一步</el-button>
			</span>
		</template>
	</el-dialog>

	<el-dialog title="健康评估登记" v-model="addhealthV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="prompt" style="color: limegreen;">
			提示：老人基本信息登记 > 家属信息登记 > 健康评估登记 > 选择床位 > 入住缴费
		</div>
		<div class="add3">
			<el-form ref="form" :model="hform" label-width="150px" size="mini" label-position="right">
				<div class="add_content3">
					<div class="add_from">
						<el-form-item label="评估人">
							<el-select v-model="hform.healthEvaluator" placeholder="请选择">
								<el-option label="2" value="2"></el-option>
								<el-option label="3" value="3"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人主要疾病">
							<el-input v-model="hform.healthDisease" placeholder="请输入内容"></el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="老人健康状态">
							<el-select v-model="hform.healthStatus" placeholder="请选择">
								<el-option label="良好" value="1"></el-option>
								<el-option label="一般" value="2"></el-option>
								<el-option label="较差" value="3"></el-option>
								<el-option label="有心理疾病" value="4"></el-option>
								<el-option label="有严重疾病" value="5"></el-option>
								<el-option label="有慢性疾病" value="6"></el-option>
								<el-option label="身体伤残" value="7"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记人">
							<el-select v-model="hform.healthRegistrant" placeholder="请选择">
								<el-option label="2" value="2"></el-option>
								<el-option label="3" value="3"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记时间">
							<input type="date" v-model="hform.healthRtime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="评估结果">
							<el-input v-model="hform.healthResult" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="hform.healthNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">取 消</el-button>
				<el-button @click="save3" size="small">保 存</el-button>
				<el-button @click="dsave3" size="small">清除保存</el-button>
				<el-button @click="last3" size="small">上一步</el-button>
				<el-button @click="next3" size="small">下一步</el-button>
			</span>
		</template>
	</el-dialog>

	<el-dialog title="选择床位/填写入住信息" v-model="addOnV" :modal="false" width="1200px" top="5vh" :before-close="close">
		<div class="prompt" style="color: limegreen;">
			提示：老人基本信息登记 > 家属信息登记 > 健康评估登记 > 选择床位 > 入住缴费
		</div>
		<div class="add4">
			<el-form ref="form" :model="onform" label-width="150px" size="mini" label-position="right">
				<div class="add_content4">
					<div class="add_from">
						<el-form-item label="入住时间">
							<input type="date" v-model="onform.onTime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="入住床位">
							<el-cascader v-model="onform.onBed" :options="options" :props="props"
								:show-all-levels="false">
							</el-cascader>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记人">
							<el-select v-model="onform.onRegistrant" placeholder="请选择">
								<el-option label="2" value="2"></el-option>
								<el-option label="3" value="3"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="登记时间">
							<input type="date" v-model="onform.onRtime" style="width: 200px;background-color: white;
								border: 1px #B3C0D1 solid;color: #8C939D;border-radius: 4px;text-align: center;" />
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="经办人">
							<el-select v-model="onform.onOperator" placeholder="请选择">
								<el-option label="2" value="2"></el-option>
								<el-option label="3" value="3"></el-option>
							</el-select>
						</el-form-item>
					</div>
					<div class="add_from">
						<el-form-item label="备注">
							<el-input v-model="onform.onNote" placeholder="请输入内容" type="textarea" :rows="3">
							</el-input>
						</el-form-item>
					</div>
				</div>
			</el-form>
		</div>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="close" size="small">取 消</el-button>
				<el-button @click="save4" size="small">保 存</el-button>
				<el-button @click="dsave4" size="small">清除保存</el-button>
				<el-button @click="last4" size="small">上一步</el-button>
				<el-button @click="addAll" size="small">提 交</el-button>
			</span>
		</template>
	</el-dialog>
</template>

<script>
	import {
		defineComponent,
		ref
	} from 'vue'
	import qs from 'qs'
	import {
		ElMessage
	} from 'element-plus'
	export default defineComponent({
		data() {
			return {
				options: [{
					value: '1',
					label: '单人间',
					children: [{
							value: '1',
							label: '101房间',
							children: [{
								value: '1',
								label: '1011号床'
							}]
						},
						{
							value: '2',
							label: '102房间',
							children: [{
								value: '2',
								label: '1021号床'
							}]
						},
						{
							value: '3',
							label: '103房间',
							children: [{
								value: '3',
								label: '1031号床'
							}]
						},
						{
							value: '10',
							label: '201房间',
							children: [{
								value: '10',
								label: '2011号床'
							}]
						},
						{
							value: '11',
							label: '202房间',
							children: [{
								value: '11',
								label: '2021号床'
							}]
						},
						{
							value: '12',
							label: '203房间',
							children: [{
								value: '12',
								label: '2031号床'
							}]
						}
					]
				}, {
					value: '2',
					label: '双人间',
					children: [{
							value: '19',
							label: '301房间',
							children: [{
								value: '19',
								label: '3011号床'
							}, {
								value: '20',
								label: '3012号床'
							}]
						},
						{
							value: '20',
							label: '302房间',
							children: [{
								value: '21',
								label: '3021号床'
							}, {
								value: '22',
								label: '3022号床'
							}]
						},
						{
							value: '21',
							label: '303房间',
							children: [{
								value: '23',
								label: '3031号床'
							}, {
								value: '24',
								label: '3032号床'
							}]
						},
						{
							value: '28',
							label: '401房间',
							children: [{
								value: '37',
								label: '4011号床'
							}, {
								value: '38',
								label: '4012号床'
							}]
						},
						{
							value: '29',
							label: '402房间',
							children: [{
								value: '39',
								label: '4021号床'
							}, {
								value: '40',
								label: '4022号床'
							}]
						},
						{
							value: '30',
							label: '403房间',
							children: [{
									value: '41',
									label: '4031号床'
								},
								{
									value: '42',
									label: '4032号床'
								}
							]
						}
					]
				}, {
					value: '3',
					label: '四人间',
					children: [{
							value: '37',
							label: '501房间',
							children: [{
								value: '55',
								label: '5011号床'
							}, {
								value: '56',
								label: '5012号床'
							}, {
								value: '57',
								label: '5013号床'
							}, {
								value: '58',
								label: '5014号床'
							}]
						}, {
							value: '38',
							label: '502房间',
							children: [{
								value: '59',
								label: '5021号床'
							}, {
								value: '60',
								label: '5022号床'
							}, {
								value: '61',
								label: '5023号床'
							}, {
								value: '62',
								label: '5024号床'
							}]
						}, {
							value: '46',
							label: '601房间',
							children: [{
								value: '91',
								label: '6011号床'
							}, {
								value: '92',
								label: '6012号床'
							}, {
								value: '93',
								label: '6013号床'
							}, {
								value: '94',
								label: '6014号床'
							}]
						}, {
							value: '47',
							label: '602房间',
							children: [{
								value: '95',
								label: '6021号床'
							}, {
								value: '96',
								label: '6022号床'
							}, {
								value: '97',
								label: '6023号床'
							}, {
								value: '98',
								label: '6024号床'
							}]
						}

					]
				}],
				props: {
					expandTrigger: 'hover'
				},
				condition: {
					what: ''
				},
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
				fform: {
					familyId: '',
					familyName: '',
					familyPhone: '',
					familyAddress: '',
					familyOrdid: '',
					familyRelation: '',
					familyAddname: '',
					familyRegistrant: '',
					familyRtime: '',
					familyNote: ''
				},
				hform: {
					healthId: '',
					healthOrdid: '',
					healthEvaluator: '',
					healthResult: '',
					healthDisease: '',
					healthStatus: '',
					healthAddname: '',
					healthRegistrant: '',
					healthRtime: '',
					healthNote: ''
				},
				onform: {
					onId: '',
					onOldid: '',
					onTime: '',
					onAddname: '',
					onBed: '',
					onRegistrant: '',
					onRtime: '',
					onOperator: '',
					onNote: ''
				},
				pageInfo1: {
					what: "",
					currentPage: 1,
					pagesize: 2,
					total: 0
				},
				what: {
					oldname: ''
				},
				addOldV: false,
				addFamilyV: false,
				addOnV: true,
				addhealthV: false,
				condata: [],
				ondata: [],
			};
		},
		methods: {
			addAll() {
				console.log(this.$store.state.userInfo.userName)
				this.onform.onBed = this.onform.onBed[2]
				console.log("入住床位")
			},
			dsave4() {
				for (var key in this.onform) {
					delete this.onform[key]
				}
				ElMessage.success({
					message: '清除成功',
					type: 'success'
				})
			},
			save4() {
				this.addOnV = false
				ElMessage.success({
					message: '保存成功',
					type: 'success'
				})
			},
			dsave3() {
				for (var key in this.hform) {
					delete this.hform[key]
				}
				ElMessage.success({
					message: '清除成功',
					type: 'success'
				})
			},
			save3() {
				this.addhealthV = false
				ElMessage.success({
					message: '保存成功',
					type: 'success'
				})
			},
			dsave2() {
				for (var key in this.fform) {
					delete this.fform[key]
				}
				ElMessage.success({
					message: '清除成功',
					type: 'success'
				})
			},
			save2() {
				this.addFamilyV = false
				ElMessage.success({
					message: '保存成功',
					type: 'success'
				})
			},
			dsave1() {
				for (var key in this.oform) {
					delete this.oform[key]
				}
				ElMessage.success({
					message: '清除成功',
					type: 'success'
				})
			},
			save1() {
				this.addOldV = false
				ElMessage.success({
					message: '保存成功',
					type: 'success'
				})
			},
			last4() {
				this.addhealthV = true
				this.addOnV = false
			},
			next3() {
				this.addhealthV = false
				this.addOnV = true
			},
			last3() {
				this.addhealthV = false
				this.addFamilyV = true
			},
			next2() {
				this.addhealthV = true
				this.addFamilyV = false
			},
			last2() {
				this.addOldV = true
				this.addFamilyV = false
			},
			next1() {
				this.addOldV = false
				this.addFamilyV = true
			},
			selectconByid() {
				var _this = this
				this.axios.get("http://localhost:8188/selectConsultingById", {
						params: this.condition
					})
					.then(function(response) {
						console.log(response.data)
						_this.oform.oldpeopleName = response.data.consultingOldname
						_this.oform.oldpeopleIdcard = response.data.consultingOldidcard
						_this.oform.oldpeopleAge = response.data.consultingOldage
						_this.oform.oldpeopleSex = response.data.consultingOldsex
						_this.oform.oldpeopleAddress = response.data.consultingOrdaddress
					}).catch(function(error) {
						console.log(error)
					})
			},
			close() {
				this.addOldV = false
				this.addFamilyV = false
				this.addOnV = false
				this.addhealthV = false
				for (var key in this.oform) {
					delete this.oform[key]
				}
			},
			indexMethod(index) {
				return index + 1;
			},
			handleSizeChange1(pagesize) {
				var _this = this
				this.pageInfo1.pagesize = pagesize
				var ps = qs.stringify(this.pageInfo1)
				console.log(ps)
				this.axios.get("http://localhost:8188/selectAllConsulting", {
						params: this.pageInfo1
					})
					.then(function(response) {
						console.log(response.data)
						_this.condata = response.data.list
					}).catch(function(error) {
						console.log(error)
					})
			},
			handleCurrentChange1(currentPage) {
				var _this = this
				this.pageInfo1.currentPage = currentPage
				var ps = qs.stringify(this.pageInfo1) // eslint-disable-line no-unused-vars
				this.axios.get("http://localhost:8188/selectAllConsulting", {
						params: this.pageInfo1
					})
					.then(function(response) {
						_this.condata = response.data.list
					}).catch(function(error) {
						console.log(error)
					})
			}
		},
		created() {
			const _this = this
			this.axios.get("http://localhost:8188/selectConsulting").then(function(response) {
				console.log(response)
				_this.condata = response.data
			}).catch(function(error) {
				console.log(error)
			})
			// this.axios.get("http://localhost:8188/selectAllBed").then(function(response) {
			// 	console.log(response)
			// 	_this.beddata = response.data
			// 	console.log(_this.beddata)
			// }).catch(function(error) {
			// 	console.log(error)
			// })
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

	.add2 {
		width: 1150px;
		height: 450px;
		margin-top: 20px;
		margin-left: 15px;
		background: white;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	.add3 {
		width: 1150px;
		height: 450px;
		margin-top: 20px;
		margin-left: 15px;
		background: white;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	.add4 {
		width: 1150px;
		height: 450px;
		margin-top: 20px;
		margin-left: 15px;
		background: white;
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
		height: 700px;
		background-color: white;
		display: flex;
		flex-wrap: wrap;
		justify-content: left;
	}

	.add_content2 {
		width: 1150px;
		height: 300px;
		background-color: white;
		display: flex;
		flex-wrap: wrap;
		justify-content: left;
	}

	.add_content3 {
		width: 1150px;
		height: 300px;
		background-color: white;
		display: flex;
		flex-wrap: wrap;
		justify-content: left;
	}

	.add_content4 {
		width: 1150px;
		height: 200px;
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
	
	.prompt{
		margin-left: 310px;
	}
</style>
