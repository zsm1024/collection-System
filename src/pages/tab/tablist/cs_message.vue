<template>
	<section  ref="abc" style="overflow-y: auto;" class="msgs" id="chatContainer">
		<el-collapse v-model="activeNames">
			<el-collapse-item name="1" title="催收状态">				
				<p>{{items.statues}}</p>				
			</el-collapse-item>	
			<el-collapse-item name="2" title="客户电话信息">	
				<el-table :data="items.persons" border  >
					<el-table-column label="操作"   align="center">
						<template scope="scope">
							<el-button :type="scope.row.edit?'success':'primary'" size="mini"  @click='phoneEdit(scope.row)' >{{scope.row.edit?'完成':'编辑'}}</el-button>
							<el-button type="danger" size="mini" v-if=" scope.row.m_path!='CMS'"  @click.native.prevent="deleteRow(scope.$index, items.persons)"> 移除</el-button>
						</template>
					</el-table-column>
					<el-table-column :prop="cols.field"   :label="cols.title" v-for="(cols, index) in cols" :key="index" align="center">
						<template scope="scope">
							<el-input  v-show="scope.row.edit" v-if="cols.field!='validity' &&scope.row.m_path=='WCMS'" size="small" v-model="scope.row[cols.field]"></el-input>
							<span v-show="scope.row.edit" v-if="cols.field!='validity' && scope.row.m_path=='CMS'" >{{ scope.row[cols.field] }}</span>
							<span v-show="!scope.row.edit"  :class="{changecolor:scope.row['validity']=='N'}" >{{ scope.row[cols.field] }}<i v-if="cols.field=='phoneNum'" class="fa fa-mobile fa-2x" style="color:#20a0ff;margin-left: 5px;cursor: pointer;" @click="ring(scope.row.phoneNum)"></i></span>
							<el-select v-show="scope.row.edit" v-if="cols.field=='validity'" v-model="scope.row[cols.field]" placeholder="请选择活动区域">
								<el-option label="Y" value="Y"></el-option>
								<el-option label="N" value="N"></el-option>
							</el-select>
						</template>
					</el-table-column>			
				</el-table>				
			</el-collapse-item>	
			 <!--  @click.native.prevent="deleteRow(scope.$index, tableData4)"-->
			<el-collapse-item name="3" title="客户地址信息">	
				<el-table :data="items.address" border >
					<el-table-column label="操作" align="center">
						<template scope="scope" >
							<el-button :type="scope.row.edit?'success':'primary'" size="mini"  @click='addressEdit(scope.row)' >{{scope.row.edit?'完成':'编辑'}}</el-button>
							<el-button type="danger" size="mini" v-if=" scope.row.m_path!='CMS'"  @click.native.prevent="deleteAdress(scope.$index, items.address)"> 移除</el-button>
						</template>
					</el-table-column>
					<el-table-column :prop="cols1.field" :label="cols1.title"  v-for="(cols1, index) in cols1" :key="index" align="center">
						<template scope="scope">
							<el-input  v-show="scope.row.edit" v-if="cols1.field!='validity' &&scope.row.m_path=='WCMS'" size="small" v-model="scope.row[cols1.field]"></el-input>
							<span v-show="scope.row.edit" v-if="cols1.field!='validity' && scope.row.m_path=='CMS'" >{{ scope.row[cols1.field] }}</span>
							<span v-show="!scope.row.edit" :class="{changecolor:scope.row['validity']=='N'}">{{ scope.row[cols1.field] }}</span>
							<el-select v-show="scope.row.edit" v-if="cols1.field=='validity'" v-model="scope.row[cols1.field]" placeholder="请选择活动区域">
								<el-option label="Y" value="Y"></el-option>
								<el-option label="N" value="N"></el-option>
							</el-select>
						</template>
					</el-table-column>			
				</el-table>				
			</el-collapse-item>	
			<el-collapse-item name="4" title="客户基本信息">
				<table>
					<tr>
						<td>客户姓名</td><td>{{items.username}}</td>
						<td>性别</td><td>{{items.sex}}</td>
						<td>单位名称</td><td class="useraddress">{{items.useraddress}}</td>
						<td>证件类别</td><td>{{items.usercardType}}</td>
						<td>证件号码</td><td>{{items.usercardNum}}</td>
					</tr>
				</table>
			</el-collapse-item>	
			<el-collapse-item name="5" title="合同基本信息">
				<table>	
					<tr>
						<td>申请号</td><td>{{items.delyNum}}</td>
						<td>合同号</td><td>{{items.contractNum}}</td>
						<td>首付比例</td><td>{{items.ShoufuRatio}}</td>
						<td>贷款金额</td><td>{{items.loan}}</td>
						<td>合同起始日</td><td>{{items.startTime}}</td>
					</tr>
					<tr>
						<td>贷款产品</td><td>{{items.loanProdut}}</td>
						<td>付款日</td><td>{{items.RepayTime}}</td>
						<td>贷款期限</td><td>{{items.loanTime}}</td>
						<td> </td><td></td>
						<td>合同终止日</td><td>{{items.endTime}}</td>
					</tr>
				</table>
			</el-collapse-item>	
			<el-collapse-item name="6" title="逾期基本信息">
				<table>
					<tr>
						<td>客户姓名</td><td>{{items.username}}</td>
						<td>性别</td><td>{{items.sex}}</td>
						<td>单位名称</td><td class="useraddress">{{items.useraddress}}</td>
						<td>证件类别</td><td>{{items.usercardType}}</td>
						<td>证件号码</td><td>{{items.usercardNum}}</td>
					</tr>
				</table>
			</el-collapse-item>	
			<el-collapse-item name="7" title="话术指引">			
				<p>{{items.remarkMessage}}</p>
			</el-collapse-item>	
			<el-collapse-item name="8" title="备注">			
				<p>{{remarkform.remark}}</p>
			</el-collapse-item>	
			<el-collapse-item name="9" title="客户信息维护">
				<el-row>
					<el-col :span="24">
						<i class="el-icon-edit" @click="remarkopen = true">备注</i>
						<i class="el-icon-message" @click="messageopen = true">短信</i>
						<i class="el-icon-upload2">附件</i>
					</el-col>
				</el-row>
				<el-row>
					<el-col :span="11">
						<el-form ref="mainform" :rules="rules" :model="mainform" label-width="80px" style="margin:20px;" >
							<div class="first">
								<el-form-item label="行动代码" prop="daima">
									<v-select v-model="mainform.daima" :options="getdaima"></v-select>
								</el-form-item>
	
							</div>
							<div class="second" style="display: flex;">
								<el-form-item label="承诺金额" prop="maney" style="width: 50%;">
									<el-input v-model="mainform.maney"></el-input>
								</el-form-item>
								<el-form-item label="承诺日期" prop="accdata" style="width: 50%;">
									<el-col :span="24">
										<el-date-picker type="date" placeholder="选择日期" v-model="mainform.accdata" style="width: 100%;"></el-date-picker>
									</el-col>
								</el-form-item>								
							</div>
							<div class="third">
								<el-form-item label="联系人" prop="name">
									<v-select v-model="mainform.name" :options="getname"></v-select>
								</el-form-item>
								<el-form-item label="联系方式"  prop="fangshi">
									<v-select v-model="mainform.fangshi" :options="getfangshi"></v-select>
								</el-form-item>								
							</div>	
							<div class="four">
								<el-form-item label="约会时间"  prop="accdatetime">
									<el-col :span="24">
										<el-date-picker type="datetime" placeholder="选择约会时间" v-model="mainform.acctime" style="width: 100%;"></el-date-picker>
									</el-col>
								</el-form-item>								
							</div>				
						</el-form>
					</el-col>
					<el-col :span="12">
						<el-form >
							<el-form-item label="备注" prop="remark">
								<el-input type="textarea" v-model="mainform.remark" ></el-input>
							</el-form-item>
							<el-form-item>
								<el-button type="primary" @click="onSubmit('mainform')">确认</el-button>
								<el-button @click="onSubmitnext('mainform')">确认&处理下一条</el-button>
							</el-form-item>
							
						</el-form>
	
					</el-col>
					
					
					
				</el-row>
			
			</el-collapse-item>	
		</el-collapse>
		<el-dialog title="备注" :visible.sync="remarkopen">
			<el-form :model="remarkform">
				<el-form-item label="备注内容" :label-width="formLabelWidth">
					<el-input type="textarea" v-model="remarkform.remark"></el-input>
				</el-form-item>
				
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button type="primary" @click="confirmremarkopen">确 定</el-button>
			</div>
		</el-dialog>
		<el-dialog title="短信" :visible.sync="messageopen">
			<el-form :model="messageform" :ref="messageform" >
				<el-form-item label="合同号" :label-width="formLabelWidth">
					<el-input v-model="messageform.hth"></el-input>
				</el-form-item>
				<el-form-item label="短信模板" :label-width="formLabelWidth">
					<v-select v-model="messageform.template" :options="getMesTemplate"></v-select>
				</el-form-item>
				<el-form-item label="手机号码" :label-width="formLabelWidth">
					<v-select v-model="messageform.phone" :options="getMesPhone"></v-select>
				</el-form-item>
				<el-form-item label="内容栏" :label-width="formLabelWidth">
						<el-input type="textarea" v-model="messageform.messagedesc"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button type="primary" @click="confirmmessage">确 定</el-button>
			</div>
		</el-dialog>		
	</section>
</template>

<script>
import { tab_view } from "@/api/api";
export default {
  data() {
    return {
    	classObject: {
    		active: true,
    		'text-danger': false
  		},
    	activeNames:["1","2","3","4","5","6",'7',"8","9"],
     	items: [],
     	height:"",
     	cols:[],
			cols1:[],
			id:this.$store.state.navTabs.tabId,

			remarkopen: false,
			messageopen: false,

			formLabelWidth: '120px',
			//备注弹出层
			remarkform: {
				remark:'',
			},
			//短信弹出层
			messageform: {
				hth:'',
				template:'',
				phone:'',
				messagedesc: '',

			},

			mainform: {
					daima: '',
					money: '',
					accdata: '',
					name: '',
					fangshi: false,
					accdatetime: '',
					remark:'',
			},
			rules: {
					daima: [
            { required: true, message: '请输入行动代码', trigger: 'blur' }
					],
					remark: [
            { required: true, message: '请填写备注', trigger: 'blur' }
          ]
			},
			getdaima: ['PTP','TSM'],
			getname: ['借款人','共借人','担保人'],
			getfangshi:["手机",'邮箱'],

			getMesTemplate: ['逾期提醒','提醒'],
			getMesPhone: ['15830903200','15603230261'],

     
     
    };
  },
  methods: {

		deleteRow(index, rows) {
			rows.splice(index, 1);
		},
		deleteAdress(index, rows) {
			rows.splice(index, 1);
		},
		ring(phoneNum){
			var divDom = this.$refs.abc;   	 
			divDom.scrollTop=divDom .scrollHeight;
		},
		phoneEdit(row){
			console.log(row.edit);
			row.edit=!row.edit;
		
		},
		
		addressEdit(row){
				
			row.edit=!row.edit;
		},
		//短信方法
		confirmmessage() {
				this.messageopen=false;
				alert('保存成功')
		},
		//备注方法
		confirmremarkopen() {
				this.remarkopen=false;
				alert('保存成功')

		},


		onSubmit(mainform) {
			this.$refs[mainform].validate((valid) => {
				if (valid) {
					alert('提交成功!');
				} else {
					console.log('error submit!!');
					return false;
				}
			});
		},
		onSubmitnext(mainform) {
			this.$refs[mainform].validate((valid) => {
				if (valid) {
					alert('提交成功，处理下一条!');
					this.getlist();
					this.$refs['mainform'].resetFields();
				} else {
					console.log('error submit!!');
					return false;
				}
			});
		},
		getlist() {
			let para = {
					id: this.id
			};
			tab_view(para).then(res => {
				let data = res.data.msg[0]
				this.items = data.data[0];
				this.items.persons = this.items.persons.map(v => {
					this.$set(v, 'edit', false)
					return v;
					
				});
				
				this.items.address = this.items.address.map(v => {
					this.$set(v, 'edit', false)
					return v
				})
				this.remarkform.remark = this.items.notice;
				this.cols=data.cols;
				this.cols1=data.cols1;   
				this.height=document.documentElement.clientHeight-400;       
			});
    },
    
  },
  mounted() {
    this.getlist();
     let h = (window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight)-155;
   this.$refs.abc.style.height= h+"px";
  }
};
</script>

<style>
	h4{background: #eef1f6;padding: 10px;border: 1px solid #dfe6ec;font-weight: bold;}
	table{width: 100%;text-align: center;}
	tr{width: 100%;}
	td{border: 1px solid #dfe6ec;height: 50px;line-height: 50px;background: #f0f0f0;}	
	.useraddress{width: 150px;}
	.el-collapse-item__header{font-size:15px;font-weight: bold;background:#dfe6ec;border: 1px solid #f0f0f0;
	};
	.abc{height: 500px!important; }
	.el-col .el-icon-edit,.el-col .el-icon-message,.el-col .el-icon-upload2{cursor: pointer; color: #20a0ff;margin-left: 5px;}
	.el-col .el-icon-upload2:hover{color: #4db3ff;}
	.el-col .el-icon-edit:hover{color: #4db3ff;}
	.el-col .el-icon-message:hover{color:#4db3ff}
	.changecolor{
		color: red;
	}
</style>