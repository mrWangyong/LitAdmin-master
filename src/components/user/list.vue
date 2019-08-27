<template>
	<el-row class="warp">
		<el-col :span="24" class="warp-breadcrum">
			<el-breadcrumb separator="/">
				<el-breadcrumb-item :to="{ path: '/' }"><b>首页</b></el-breadcrumb-item>
				<el-breadcrumb-item>用户列表</el-breadcrumb-item>
			</el-breadcrumb>
		</el-col>

		<el-col :span="24" class="warp-main" v-loading="loading" element-loading-text="拼命加载中">
			<!--工具条-->
			<el-col :span="24" class="toolbar" style="padding-bottom: 0px;">
				<el-form :inline="true">
					<el-form-item>
						<el-input v-model="userName" placeholder="姓名" style="min-width: 240px;"  @keyup.native="userList"></el-input>
					</el-form-item>
					<el-form-item>
						<el-button type="primary" @click="userList">查询</el-button>
					</el-form-item>
				</el-form>
			</el-col>

			<!--列表-->
			<el-table :data="users" highlight-current-row v-loading="loading" style="width: 100%;">
				<el-table-column type="index" label="序号" width="60" >
				</el-table-column>
				<el-table-column prop="companyName" label="公司" min-width="120">
				</el-table-column>
				<el-table-column prop="marketDepartment" label="客户营销部" min-width="120">
				</el-table-column>
				<el-table-column prop="clientServiceDepartment" label="客户服务部" min-width="120">
				</el-table-column>
				<el-table-column prop="userName" label="姓名" min-width="100">
				</el-table-column>
				<el-table-column prop="nickName" label="电话" min-width="160">
				</el-table-column>
				<el-table-column prop="loginName" label="登录名">
				</el-table-column>
				<el-table-column
			      fixed="right"
			      label="操作"
			      width="120">
			      <template slot-scope="scope">
			        <el-button
			          @click.native.prevent="details(scope.$index, users)"
			          type="text"
			          size="small">
			          	详情
			        </el-button>
			      </template>
    			</el-table-column>
  </el-table>
					
					
				</el-table-column>
			</el-table>
			
			<center class="block page">
			    <el-pagination
			      background
			      @current-change="handleCurrentChange"
			      :page-size="pageSize"
			      layout="total, prev, pager, next"
			      :total="userCount">
			    </el-pagination>
			</center>
			
		</el-col>
	</el-row>
</template>

<script>
	import Global from '../../components/Global';
	import axios from 'axios';
	

	export default {
		data() {
			return {
				userName : '',
				loading: false,
				users: [],
				total: 0,
				page: 1,
				limit: 10,
				loading: false,
				pageNum : 1,
				pageSize : 9,
				userCount : 0,
			}
		},
		methods: {
			userList() {
				var that = this;
				let userParams = {
					userName: that.userName,
				};
				axios({
						method: "post", 
						url: Global.serverSrc+"/user/getUserlist/"+that.pageNum+"/"+that.pageSize+"/44",
						data: JSON.stringify(userParams),
						headers: {
							"Content-Type": "application/json;charset=UTF-8",
							"Authorization" : localStorage.getItem("token")
						}
					})
					.then(function(res) {
						console.log(res.data);
						if(res.data.resultCode == 200) {
							that.users = res.data.userlist;
						} else {
							that.$message.error({
								showClose: true,
								message: res.data.message,
								duration: 2000
							});
						}
					})
					.catch(function(err) {
						if(err.response) {
							console.log(err.response)
						}
					})
			},
			userListCount() {
				var that = this;
				let userParams = {
					userName: that.userName,
				};
				axios({
						method: "post", 
						url: Global.serverSrc+"/user/countUser/44",
						data: JSON.stringify(userParams),
						headers: {
							"Content-Type": "application/json;charset=UTF-8",
							"Authorization" : localStorage.getItem("token")
						}
					})
					.then(function(res) {
						console.log(res.data);
						if(res.data.resultCode == 200) {
							that.userCount = res.data.count;
						} else {
							that.$message.error({
								showClose: true,
								message: res.data.message,
								duration: 2000
							});
						}
					})
					.catch(function(err) {
						if(err.response) {
							console.log(err.response)
						}
					})
			},
		    handleCurrentChange(val) {
		    	var that = this ;
		    	that.pageNum = val;
		    	that.userList();
		    },
		    details(index,user){
		    	console.log(index);
		    	console.log(user[index]);
		    	
		    	this.$router.push({ name: 'UserDetails', params: { UserDetails: user[index] }});
//		    	this.$router.push('/userDetails'); //用go刷新
		    },
		},
		mounted() {
			this.userList();
			this.userListCount();
		}
	}
</script>

<style scoped>
	.page{
		margin-top: 20px;
	}
</style>