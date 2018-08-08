<template>
	<div class="UsersManageMent">
		<el-row :gutter="24" style="padding: 20px 0 0 0;">
			<el-tabs v-model="activeName" type="card" @tab-click="handleClick">
				<el-tab-pane class="aaa" name="first">
					<span slot="label" style="font-size: 16px;">客服管理</span>
					<el-col :span="2">
						<el-button type="primary" size="mini" @click="NewCustomServiceAdd()">
							<i class="el-icon-plus"></i>添加客服
						</el-button>
					</el-col>
					<el-col :span="2">
						<el-button type="primary" size="mini" @click="AllotGroup()">
							<i class="el-icon-plus"></i>分配组别
						</el-button>
					</el-col>

					<el-col :offset="14" :span="6">
						<el-input type="text" size="mini" prefix-icon="el-icon-search" v-model="search" placeholder="租户名称"></el-input>
					</el-col>
					<el-col :span="24" style="min-height: 365px;">
						<el-table ref="multipleTable" @selection-change="handleSelectionChange" :data="tableData" style="width: 100%" size="mini" :border="true">
              <el-table-column  width="50px" align="center">
                <template slot-scope="scope" >
                  <el-radio class="aaadd" :label="scope.$index" v-model="radios" @change.native="getCurrentRow(scope.$index,scope.row)"></el-radio>
                </template>
              </el-table-column>
							<el-table-column label="租户名称" width="200px" prop="tenanTenanName">

							</el-table-column>
							<el-table-column label="客服名称" min-width="100px" prop="customerName">

							</el-table-column>
							<el-table-column label="用户名" width="140px" prop="userName">

							</el-table-column>
              <el-table-column label="昵称" width="140px" prop="userNickname">

              </el-table-column>
							<el-table-column label="性别"  width="50px" align="center">
                <template slot-scope="scope">
                  <span>{{ scope.row.userSex | sexFilter}}</span>
                </template>
							</el-table-column>
							<el-table-column label="手机号" prop="userPhone" width="130px" align="center">

							</el-table-column>
							<el-table-column label="微信号" prop="userWechatNumber" width="130px" align="center">

							</el-table-column>
              <el-table-column label="帐号状态" width="100px" align="center">
                <template slot-scope="scope">
                  <el-button size="mini" type="danger" plain v-if="scope.row.userEnable==0" @click="AccountFreeze(scope.$index,scope.row)">冻结</el-button>
                  <el-button size="mini" type="primary" plain v-if="scope.row.userEnable==1" @click="AccountFreeze(scope.$index,scope.row)">恢复</el-button>
                </template>
              </el-table-column>
							<el-table-column label="操作" width="200px" align="center">
								<template slot-scope="scope">
									<el-button size="mini" type="primary" plain @click="CustomServiceUpdata(scope.$index,scope.row)">编辑</el-button>
									<el-button size="mini" type="danger" plain @click.native.prevent="handleDelete(scope.$index,scope.row)">删除</el-button>
								</template>
							</el-table-column>
						</el-table>
					</el-col>
					<el-col :offset="10" >
						<el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page.sync="currentPage" :page-sizes="[10,20,30,50]" :page-size="pagesize" layout="total, sizes, prev, pager, next, jumper" :total="totalPage">
						</el-pagination>
					</el-col>
				</el-tab-pane>
				<el-tab-pane name="second">
					<span slot="label" style="font-size: 16px;">客服组管理</span>
					<el-col :span="2">
						<el-button type="primary" size="mini" @click="AllotRole()">
							<i class="el-icon-plus"></i>分配角色
						</el-button>
					</el-col>
					<el-col :span="3">
						<el-button type="primary" size="mini" @click="NewGroupAdd()">
							<i class="el-icon-plus"></i>新增客服组别
						</el-button>
					</el-col>

					<el-col :offset="13" :span="6">
						<el-input type="text" size="mini" prefix-icon="el-icon-search" v-model="search2" placeholder="租户名称"></el-input>
					</el-col>
					<el-col :span="24" style="min-height: 365px;">
						<el-table ref="multipleTable" @selection-change="handleSelectionChange" :data="tableData2" style="width: 100%" size="mini" :border="true">
              <el-table-column  width="50px" align="center">
                <template slot-scope="scope" >
                  <el-radio class="aaadd" :label="scope.$index" v-model="groupRadios" @change.native="getGroupRow(scope.$index,scope.row)"></el-radio>
                </template>
              </el-table-column>
							<el-table-column label="租户名称" width="180px" prop="tenanId">

							</el-table-column>
							<el-table-column label="用户组别名称" width="180px" prop="groupName">

							</el-table-column>
							<el-table-column label="描述" prop="intro">

							</el-table-column>
							<!--<el-table-column label="组别角色" prop="UserGroupGescribe">-->

							<!--</el-table-column>-->
							<!--<el-table-column label="创建时间" width="100px" prop="FoundDate" align="center">-->

							<!--</el-table-column>-->
							<el-table-column label="操作" width="220px" align="center">
								<template slot-scope="scope">
									<el-button size="mini" type="primary" plain @click="CustomServiceGroupUpdata(scope.$index,scope.row)">编辑</el-button>
									<el-button size="mini" type="primary" plain @click="ThisGroupShow(scope.$index,scope.row)">展示</el-button>
									<el-button size="mini" type="danger" plain @click.native.prevent="handleDelete(scope.$index,scope.row)">删除</el-button>
								</template>
							</el-table-column>
						</el-table>
					</el-col>
					<el-col :offset="10" >
						<el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page.sync="currentPage2" :page-sizes="[10,20,30,50]" :page-size="pagesize2" layout="total, sizes, prev, pager, next, jumper" :total="totalPage2">
						</el-pagination>
					</el-col>
				</el-tab-pane>
			</el-tabs>

		</el-row>

		<!--添加客服-->
		<!--<el-dialog title="添加客服" :visible.sync="NewCustomServiceShow" width="300px" top="10vh">-->
			<!--<el-checkbox :indeterminate="isIndeterminate3" v-model="CustomServiceCheckAll" @change="CustomServiceAllChange">全选</el-checkbox>-->
			<!--<div style="margin: 15px 0;"></div>-->
			<!--<el-checkbox-group v-model="checkedService" @change="CustomServiceChange">-->
				<!--<div v-for="v in service">-->
					<!--<el-checkbox :label="v" :key="v">{{v}}</el-checkbox>-->
				<!--</div>-->
			<!--</el-checkbox-group>-->
			<!--<div slot="footer" class="dialog-footer">-->
				<!--<el-button @click="NewUser()">取 消</el-button>-->
				<!--<el-button type="primary" @click="NewCustomServiceAddChild()">确 定</el-button>-->
			<!--</div>-->
		<!--</el-dialog>-->
    <el-dialog title="添加客服" :visible.sync="NewCustomServiceShow" top="10vh">
      <el-col :span="24" style="min-height: 365px;">
        <el-table ref="multipleTable" @selection-change="handleSelectionChange" :data="tableData3" style="width: 100%" size="mini" :border="true">
          <el-table-column type="selection" width="50px" align="center">

          </el-table-column>

          <el-table-column label="租户ID" width="100px" prop="tenan_name">

          </el-table-column>
          <el-table-column label="用户名" prop="name">

          </el-table-column>
          <el-table-column label="用户昵称" min-width="110px" prop="nickname">

          </el-table-column>
          <el-table-column label="性别" width="50px" align="center">
            <template slot-scope="scope">
              <span>{{ scope.row.sex | sexFilter}}</span>
            </template>
          </el-table-column>
          <el-table-column label="手机号" prop="phone" width="100px" align="center">

          </el-table-column>
          <el-table-column label="微信号" prop="wechatNumber" width="100px" align="center">

          </el-table-column>
        </el-table>
      </el-col>
      <el-col :offset="10" >
        <el-pagination background @size-change="handleSizeChange1" @current-change="handleCurrentChange1" :current-page.sync="currentPage3" :page-sizes="[10,20,35,50]" :page-size="pagesize3" layout="total, sizes, prev, pager, next, jumper" :total="totalPage3">
        </el-pagination>
      </el-col>


      <div slot="footer" class="dialog-footer">
        <el-button @click="NewCustomServiceShow = false">取 消</el-button>
        <el-button type="primary" @click="NewCustomServiceAddChild()">确 定</el-button>
      </div>
    </el-dialog>
		<!--当前客服信息编辑-->
		<el-dialog title="编辑客服" :visible.sync="CustomServiceUpdataShow" width="500px" top="5vh">
			<el-form :model="updateForm" size="mini" ref="updateForm">
				<el-form-item label="租户名称" :label-width="formLabelWidth" required>
					<el-input v-model="updateForm.tenanTenanName" placeholder="请选择租户ID" auto-complete="off"></el-input>
				</el-form-item>
        <el-form-item label="客服名" :label-width="formLabelWidth" required>
          <el-input v-model="updateForm.customerName" placeholder="请输入客服名称" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="用户名" :label-width="formLabelWidth" required>
          <el-input v-model="updateForm.userName" placeholder="请输入用户名" auto-complete="off"></el-input>
        </el-form-item>
				<el-form-item label="昵称" :label-width="formLabelWidth" required>
					<el-input v-model="updateForm.userNickname" placeholder="请输入用户昵称" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="手机号" :label-width="formLabelWidth" required>
					<el-input v-model="updateForm.userPhone" placeholder="请输入手机号" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="微信号" :label-width="formLabelWidth" required>
					<el-input v-model="updateForm.userWechatNickname" placeholder="请输入微信号" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="QQ号" :label-width="formLabelWidth" required>
					<el-input v-model="updateForm.userQq" placeholder="请输入QQ号" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="邮箱" :label-width="formLabelWidth" required>
					<el-input v-model="updateForm.userEmail" placeholder="请输入邮箱" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="性别" :label-width="formLabelWidth" required>
					<el-select v-model="updateForm.userSex" placeholder="请选择性别">
						<el-option value="0" lable="男"></el-option>
						<el-option value="1" lable="女"></el-option>
					</el-select>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="CustomServiceUpdataShow=false">取 消</el-button>
				<el-button type="primary" @click="NewCustomServiceUpdate('updateForm')">确 定</el-button>
			</div>
		</el-dialog>

		<!--分配角色-->
		<el-dialog title="分配组别" width="300px" :visible.sync="AllotGroupShow" top="20vh">
			<el-checkbox :indeterminate="isIndeterminate2" v-model="AllotGroupCheckAll" @change="AllotGroupAllChange">全选</el-checkbox>
			<div style="margin: 15px 0;"></div>
			<el-checkbox-group v-model="checkedGroup" @change="AllotGroupChange">
				<div v-for="v in group">
					<el-checkbox :label="v.id" :key="v.id">{{v.groupName}}</el-checkbox>
				</div>
			</el-checkbox-group>
			<div slot="footer" class="dialog-footer">
				<el-button @click="AllotGroupShow = false">取 消</el-button>
				<el-button type="primary" @click="AllotGroupUpdata()">确 定</el-button>
			</div>
		</el-dialog>

		<!--分配角色-->
		<el-dialog title="分配角色" width="300px" :visible.sync="AllotRoleUpdateShow" top="20vh">
			<!--<el-checkbox :indeterminate="isIndeterminate1" v-model="AllotRoleCheckAll" @change="AllotRoleAllChange">全选</el-checkbox>-->
			<div style="margin: 15px 0;"></div>
			<el-checkbox-group v-model="checkedService" @change="AllotRoleChange">
				<div v-for="x in service">
					<el-checkbox :label="x.id" :key="x.id">{{x.name}}</el-checkbox>
				</div>
			</el-checkbox-group>
			<div slot="footer" class="dialog-footer">
				<el-button @click="AllotRoleUpdateShow = false">取 消</el-button>
				<el-button type="primary" @click="AllotRoleUpdate()">确 定</el-button>
			</div>
		</el-dialog>

		<el-dialog title="新增组别" :visible.sync="NewGroupShow" top="10vh">
			<el-form :model="form" size="mini" :rules="rules" ref="form">
        <el-form-item label="租户名称" label-width="100px" prop="clientTenanId">
          <el-select v-model="form.clientGroupTenanId" placeholder="请选择租户">
            <el-option v-for="v in form.tenanList"  :key="v.id" :value="v.id" :label="v.tenanName"></el-option>
          </el-select>
        </el-form-item>
				<el-form-item label="客服组名称" label-width="100px">
					<el-input v-model="form.groupName" placeholder="请输入客服组名称" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="描述" label-width="100px">
					<el-input v-model="form.intro" placeholder="请输入租户描述" auto-complete="off"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="NewGroupShow=false">取 消</el-button>
				<el-button type="primary" @click="NewGroupUpdate('form')">确 定</el-button>
			</div>
		</el-dialog>

		<!--组别编辑-->
		<el-dialog title="客服组编辑" :visible.sync="CustomServiceGroupUpdataShow" top="10vh">
			<el-form :model="updateForm" size="mini" ref="updateForm" :rules="rules">
				<el-form-item label="客服组名称" label-width="100px">
					<el-input v-model="updateForm.groupName" placeholder="请输入租户ID" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="描述" label-width="100px">
					<el-input v-model="updateForm.intro" placeholder="请输入租户ID" auto-complete="off"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click="CustomServiceGroupUpdataShow=false">取 消</el-button>
				<el-button type="primary" @click="NewCustomServiceGroupUpdata('updateForm')">确 定</el-button>
			</div>
		</el-dialog>
		<!--组别展示-->
		<el-dialog :title="title" :visible.sync="GroupShow" top="10vh">
			<el-table :data="tableData" border style="width: 100%" size="mini">
				<el-table-column prop="TenantId" label="日期" width="180">
				</el-table-column>
				<el-table-column prop="name" label="姓名" width="180">
				</el-table-column>
				<el-table-column prop="address" label="地址">
				</el-table-column>
			</el-table>
			<div slot="footer" class="dialog-footer">
				<el-button @click="GroupShow=false">取 消</el-button>
			</div>
		</el-dialog>
	</div>
</template>

<script>
	import { mapState } from 'vuex'
  import { formatDate,Url } from '@/untils/date.js'
	export default {
		data() {
			return {
				title: "",
				tableData: [],
				tableData2: [],
        tableData3: [],
				//添加客服显示
				NewCustomServiceShow: false,
				//分配组别显示
				AllotGroupShow: false,
				//分配角色显示
				AllotRoleUpdateShow: false,
				//新增组别显示
				NewGroupShow: false,
				//当前组别展示
				GroupShow: false,
				//当前客服编辑显示隐藏
				CustomServiceUpdataShow:false,
				//当前客服组别编辑显示隐藏
				CustomServiceGroupUpdataShow:false,
				form: {},
				updateForm: {},
				//表单label宽度
				formLabelWidth: '80px',

				currentIndex: "",
				//选择框当前值
				multipleSelection: [],
				//tab标签页显示项
				activeName: 'first',
				//
				AllotRoleCheckAll: false,
				checkedCities: [],
				cities: ['超级管理员', '租户管理员', '租户用户', '9527'],
				isIndeterminate1: true,
				//组别分配
				AllotGroupCheckAll: false,
				checkedGroup: [],
				group: ['客服1组', "客服2组", "客服3组", "客服4组"],
				isIndeterminate2: true,
				//角色分配
				CustomServiceCheckAll: false,
				service: ['客服1', "客服2", "客服3", "客服4"],
				checkedService: [],
				isIndeterminate3: true,

        tabIndex:0,
				search: '',
				search2: '',
				currentPage: 0,
				pagesize: 10,
        totalPage:0,
				currentPage2: 0,
				pagesize2: 10,
        totalPage2:0,
        currentPage3: 0,
        pagesize3: 10,
        totalPage3:0,
        currentLine:'',
        groupId:'',
        radios:null,
        groupRadios:null,
        rules:{

        }
			}
		},
		methods: {
      getCurrentRow(index,row){
        this.currentLine=row.customerId;
      },
      getGroupRow(index,row){
        this.groupId=row.id;
      },
			//组别编辑
			CustomServiceGroupUpdata(index,row){
				this.title = row.UserGroupName;
				this.updateForm = Object.assign({}, row);
				this.currentIndex = index;
				this.CustomServiceGroupUpdataShow =true;
			},
			NewCustomServiceGroupUpdata(){
				this.tableData2[this.currentIndex] = this.updateForm;
				this.$set(this.tableData2, this.currentIndex, this.tableData2[this.currentIndex])
				this.CustomServiceGroupUpdataShow =false;
			},
			//客服人员编辑
			CustomServiceUpdata(index,row){
				this.updateForm = Object.assign({}, row);
				this.currentIndex = index;
				this.CustomServiceUpdataShow = true;
			},
			NewCustomServiceUpdate(formName){
        this.$refs[formName].validate(valid=>{
          if(valid){
            this.$axios.put(Url+"/customer/updateCustomer",this.updateForm).then(response => {
              if(response.data.code===0){
                this.$message.success(response.data.message);
                this.response();
                this.CustomServiceUpdataShow = false;
              }else{
                this.$message.error(response.data.message);
              }
            }, response => {
              // 错误回调
            })
          }
        })
				// this.tableData[this.currentIndex] = this.updateForm;
				// this.$set(this.tableData, this.currentIndex, this.tableData[this.currentIndex])
			},
			//用户组展示
			ThisGroupShow(index, row) {
				this.GroupShow = true;
				console.log(row)
				this.title = row[index].UserGroupName
			},
			//添加客服
			NewCustomServiceAdd() {
        this.$axios.get(Url+"/user/getUserAll",{tenanId:this.tenanId,start:this.currentPage3,length:this.pagesize3,flag:1,id:1}).then(response => {
          // 成功回调
          if(response.data.code===0){
            this.tableData3 = response.data.data[0].data;
            this.totalPage3=response.data.data[0].recordsTotal;
            this.NewCustomServiceShow = true;
          }
        }, response => {
          // 错误回调
        });
			},
			NewCustomServiceAddChild() {
        let custom=[];
        for(let i=0;i<this.multipleSelection.length;i++){
          custom.push(this.multipleSelection[i].id);
        }
        let customId=custom.toString();
        this.$axios.post(Url+"/customer/saveCustomer",{customerUserIds:customId,customerCreate:1}).then(response => {
          if(response.data.code===0){
            this.response();
            this.$message.success(response.data.message);
            this.NewCustomServiceShow=false;
          }else{
            this.$message.error(response.data.message);
            this.NewCustomServiceShow=false;
          }
        });
			},
			CustomServiceAllChange(val) {
				this.checkedService = val ? this.service : [];
				this.isIndeterminate3 = false;
			},
			CustomServiceChange(value) {
				let checkedCount = value.length;
				this.CustomServiceCheckAll = checkedCount === this.service.length;
				this.isIndeterminate3 = checkedCount > 0 && checkedCount < this.service.length;
			},
			//新增组
			NewGroupAdd() {
				this.form = {
          groupName: "",
          intro: "",
          tenanId:1
				};
        this.$axios.get(Url + "/tenan/selectAllTenan",{tenanId:1,flag: 1}).then(response => {
          if(response.data.code === 0) {
            this.form.tenanList = response.data.data;
            this.NewGroupShow = true;
          }
        }, response => {
          // 错误回调
          this.$message.error(response.data.message);
        });
			},
			NewGroupUpdate(formName) {
        delete(this.form.tenanList);
        this.$refs[formName].validate(valid=> {
              if (valid) {
        this.$axios.post(Url+"/customerGroup/createUserGroup",this.form).then(response => {
              if(response.data.code===0){
                this.$message.success(response.data.message);
                this.response1();
                this.NewGroupShow = false;
              }else{
                this.$message.error( response.data.message);
              }
            }, response => {
              // 错误回调
              this.$message.error( response.data.message);
            });
          }
        });
			},
			//分配组别
			AllotGroup() {
        if(this.currentLine === "") {
          this.$message.warning('请选择你要分配的用户');
          return;
        }else{
          this.$axios.get(Url+"/customer/listCustomerGroup",{customerId:this.currentLine}).then(response => {
            if(response.data.code===0){
              this.group=response.data.data;
              for (let i=0;i<response.data.data.length;i++){
                if(response.data.data[i].flg===0){
                  this.checkedGroup .push(response.data.data[i].id);
                }
              }
              this.AllotGroupShow = true;
            }
          });
        }
			},
			AllotGroupUpdata() {
        let groupId= this.checkedGroup.toString();
        this.$axios.post(Url+"/customer/saveCustomerGroupRelation",{customerId:this.currentLine,userGroupIds:groupId,creater:this.currentLine}).then(response => {
          if(response.data.code===0){
            this.$message.success( response.data.message);
            this.AllotGroupShow = false;
            this.toggleSelection();
            this.checkedGroup=[];
          }else{
            this.AllotGroupShow = false;
            this.$message.error( response.data.message);
            this.checkedGroup=[];
          }
        });
			},
			//组别分配复选获取
			AllotGroupAllChange(val) {
				this.checkedGroup = val ? this.group : [];
				this.isIndeterminate2 = false;
			},
			AllotGroupChange(value) {
				let checkedCount = value.length;
				this.AllotGroupCheckAll = checkedCount === this.group.length;
				this.isIndeterminate2 = checkedCount > 0 && checkedCount < this.group.length;
			},
			//删除当前客服，客服组
      handleDelete(index,row,tabIndex) {
        this.$confirm('此操作将永久删除该用户, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          if(this.tabIndex===0){
            this.$axios.del(Url+"/customer/deleteCustomer",{customerId:row.customerId}).then(response => {
              if(response.data.code===0){
                this.response();
                this.$message.success(response.data.message );
              }else{
                this.$message.error(response.data.message );
              }
            }, response => {
              // 错误回调
              this.$message.error(response.data.message);
            });
          }else{
            this.$axios.del(Url+"/customerGroup/delUserGroup",{id:row.id,tenanId:row.tenanId}).then(response => {
              if(response.data.code===0){
                this.response1();
                this.$message.success(response.data.message );
              }else{
                this.$message.error(response.data.message );
              }
            }, response => {
              // 错误回调
              this.$message.error(response.data.message);
            });
          }
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
      },
      //冻结、恢复当前用户按钮
      AccountFreeze(index, row) {
        this.$confirm('此操作将冻结当前用户, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$axios.put(Url+"/customer/updateCustomerFreeze",{customerId:row.customerId,userEnable:row.userEnable}).then(response => {
            // 成功回调
            if(response.data.code===0){
              if(row.userEnable===0){
                row.userEnable=1;
              }else{
                row.userEnable=0;
              }
              this.$message({
                type: 'success',
                duration: 1000,
                message: response.data.message
              });
            }else{
              this.$message({
                type: 'error',
                duration: 1000,
                message: response.data.message
              });
            }
          }, response => {
            // 错误回调
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            duration: 1000,
            message: '已取消操作'
          });
        });
      },
			//分配角色
			AllotRole() {
				if(this.groupId === "") {
					this.$message({
						message: '请选择你想要分配的客服',
						type: 'warning'
					});
					return
				};
        this.$axios.get(Url+"/customerGroup/getUserGroupRole",{id:this.groupId,tenanId:this.tenanId}).then(response => {
          if(response.data.code===0){
            this.service=response.data.data;
            for (let i=0;i<response.data.data.length;i++){
              if(response.data.data[i].flg===0){
                this.checkedService .push(response.data.data[i].id);
              }
            }
           this.AllotRoleUpdateShow = true;
          }
        });
			},
			AllotRoleUpdate() {
        let roles= this.checkedService.toString();
        this.$axios.post(Url+"/customerGroup/allotUserGroupRole",{groupid:this.groupId,roleids:roles,creater:1}).then(response => {
          if(response.data.code===0){
            this.$message.success( response.data.message);
            this.AllotRoleUpdateShow = false;
            this.checkedService=[];
            this.toggleSelection();
          }else{
            this.AllotRoleUpdateShow = false;
            this.checkedService=[];
            this.$message.error( response.data.message);
          }
        });
			},
			//角色分配复选获取
			AllotRoleAllChange(val) {
				this.checkedService = val ? this.service : [];
				this.isIndeterminate3 = false;
			},
			AllotRoleChange(value) {
				let checkedCount = value.length;
				this.AllotRoleCheckAll = checkedCount === this.service.length;
				this.isIndeterminate3 = checkedCount > 0 && checkedCount < this.service.length;
			},

			//重置表格
			toggleSelection(rows) {
				if(rows) {
					rows.forEach(row => {
						this.$refs.multipleTable.toggleRowSelection(row);
					});
				} else {
					this.$refs.multipleTable.clearSelection();
				}
			},
			//			取消
      //取消,编辑用户，删除用户
      // NewUser(formName) {
      //   // this.$message('已取消');
      //   this.$refs[formName].resetFields();
      //   this.NewCustomService = false;
      //   this.CustomServiceUpdataShow = false;
      // },

			//tab切换
			handleClick(tab, event) {
        this.tabIndex=tab.index;
        if(tab.index==="1"){
          this.$axios.get(Url+"/customerGroup/getAllUserGroup",{tenanId:this.tenanId,start:0,length:10,flag:1}).then(response => {
            // 成功回调
            if(response.data.code===0){
              this.tableData2 = response.data.data[0].data;
              this.totalPage2=response.data.data[0].recordsTotal;
            }
          }, response => {
            // 错误回调
          })
        }
			},
			handleSelectionChange(val) {
				this.multipleSelection = val;
			},
      handleSizeChange(val,tabIndex) {
        if(this.tabIndex===0){
          this.$axios.get(Url+"/customer/listCustomer",{tenanId:this.tenanId,start :this.currentPage-1,length:val,flag:1}).then(response => {
            // 成功回调
            if(response.data.code===0){
              this.tableData = response.data.data[0].data;
              this.totalPage=response.data.data[0].recordsTotal;
              this.pagesize = val;
            }
          }, response => {
            // 错误回调
          })
        }else{
          this.$axios.get(Url+"/customer/listCustomerGroup",{tenanId:this.tenanId,start:this.currentPage2-1,length:val,flag:1}).then(response => {
            // 成功回调
            if(response.data.code===0){
              this.pagesize2 = val;
              this.tableData2 = response.data.data[0].data;
              this.totalPage2=response.data.data[0].recordsTotal;
            }
          }, response => {
            // 错误回调
          })
        }
      },
      handleCurrentChange(val,tabIndex) {
        if(this.tabIndex===0){
          this.$axios.get(Url+"/customer/listCustomer",{tenanId:this.tenanId,start:val-1,length:this.pagesize,flag:1}).then(response => {
            // 成功回调
            if(response.data.code===0){
              this.currentPage = val;
              this.tableData = response.data.data[0].data;
              this.totalPage=response.data.data[0].recordsTotal;
            }
          }, response => {
            // 错误回调
          })
        }else{
          this.$axios.get(Url+"/customer/listCustomerGroup",{tenanId:this.tenanId,start:val-1,length:this.pagesize2,flag:1}).then(response => {
            // 成功回调
            if(response.data.code===0){
              this.currentPage2 = val;
              this.tableData2= response.data.data[0].data;
              this.totalPage2=response.data.data[0].recordsTotal;
            }
          }, response => {
            // 错误回调
          })
        }
      },
      response(){
        this.$axios.get(Url+"/customer/listCustomer",{tenanId:this.tenanId,start:this.currentPage,length:this.pagesize,flag:1}).then(response => {
          // 成功回调
          if(response.data.code===0){
            this.tableData = response.data.data[0].data;
            this.totalPage=response.data.data[0].recordsTotal;
          }
        }, response => {
          // 错误回调
        })
      },
      response1(){
        this.$axios.get(Url+"/customerGroup/getAllUserGroup",{tenanId:this.tenanId,start:this.currentPage2,length:this.pagesize2,flag:1}).then(response => {
          // 成功回调
          if(response.data.code===0){
            this.tableData2 = response.data.data[0].data;
            this.totalPage2=response.data.data[0].recordsTotal;
          }
        }, response => {
          // 错误回调
        })
      },
      handleSizeChange1(val) {
        this.$axios.get(Url+"/user/getUserAll",{tenanId:this.tenanId,start:this.currentPage-1,length:val,flag:1}).then(response => {
          if(response.data.code===0){
            this.tableData3 = response.data.data[0].data;
            this.totalPage3=response.data.data[0].recordsTotal;
            this.pagesize3 = val;
          }
        }, response => {
          // 错误回调
        })
      },
      handleCurrentChange1(val) {
        this.$axios.get(Url+"/user/getUserAll",{tenanId:this.tenanId,start:val-1,length:this.pagesize,flag:1}).then(response => {
          // 成功回调
          if(response.data.code===0){
            this.currentPage3 = val;
            this.tableData3 = response.data.data[0].data;
            this.totalPage3=response.data.data[0].recordsTotal;
          }
        }, response => {
          // 错误回调
        })
      },
		},
    created(){
      this.response();
    },
    filters: {
      formatDate(time) {
        var date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      },
      sexFilter(num) {
        if (num === 0) {
          return num = "男"
        } else if (num === 1) {
          return num = "女"
        }
      }
    },
      enableFilter(num){
        if(num===0){
          return num = "正常"
        }else if(num===1){
          return num = "冻结"
        }
      },
		computed: {
			tables() {
				var search = this.search;
				if(search) {
					return this.tableData.filter(function(dataNews) {
						return Object.keys(dataNews).some(function(key) {
							return String(dataNews[key]).toLowerCase().indexOf(search) > -1;
						})
					})

				}
				return this.tableData

			},
			tables2() {
				var search = this.search2;
				if(search) {
					return this.tableData2.filter(function(dataNews) {
						return Object.keys(dataNews).some(function(key) {
							return String(dataNews[key]).toLowerCase().indexOf(search) > -1;
						})
					})

				}
				return this.tableData2

			},
		}
	}
</script>

<style lang="scss" scoped>
	.UsersManageMent {
		width: 83.5%;
		min-height: 86.5vh;
		max-height: 100%;
		background: white;
		.PaginationButtom {
			padding: 15px 0;
		}
		.el-checkbox__label {
			display: inline-block;
			width: 100px !important;
		}
	}
</style>
