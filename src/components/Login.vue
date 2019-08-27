<template>
	<el-form ref="AccountFrom" :model="account" :rules="rules" label-position="left" label-width="0px" class="demo-ruleForm login-container">
		<h3 class="title">管理员登录</h3>
		<el-form-item prop="username">
			<el-input type="text" v-model="account.username" auto-complete="off" placeholder="账号"></el-input>
		</el-form-item>
		<el-form-item prop="pwd">
			<el-input type="text" v-model="account.pwd" auto-complete="off" placeholder="密码"></el-input>
		</el-form-item>
		<!--<el-checkbox v-model="checked" checked class="remember">记住密码</el-checkbox>-->
		<el-form-item style="width:100%;">
			<el-button type="primary" style="width:100%;" @click.native.prevent="handleLogin" :loading="loading">登录</el-button>
		</el-form-item>
	</el-form>
</template>

<script>
	import API from '../api/api_user';
	import axios from 'axios';
	import Global from '../components/Global';

	export default {
		data() {
			return {
				severApi:Global.serverSrc,
				loading: false,
				account: {
					username:"5",
					pwd: '5'
				},
				rules: {
					username: [{
							required: true,
							message: '请输入账号',
							trigger: 'blur'
						},
					],
					pwd: [{
							required: true,
							message: '请输入密码',
							trigger: 'blur'
						},
					]
				},
				checked: true
			};
		},
		methods: {
			handleLogin() {
				var that = this ;
				let loginParams = {
					loginName: this.account.username,
					password: this.account.pwd
				};
				if(!loginParams.loginName || !loginParams.password){
					return ;
				}
				axios({
				 	method:"post",
				    url:Global.serverSrc+"/user/login",
				    data: loginParams,
				    headers : {
						"Content-Type":"application/json;charset=UTF-8"
				    }
				})
				.then(function(res) {
						console.log(res.data);
						if(res.data.resultCode == 200){
//							localStorage.setItem('access-user', JSON.stringify(res.data));
							localStorage.token = res.data.token;
							localStorage.userName = res.data.userName;
							localStorage.companyName = res.data.companyName;
							localStorage.userLevel = res.data.userLevel;
							that.$router.push({path: '/'});
						}else {
							that.$message.error({showClose: true, message:res.data.message, duration: 2000});
						}
				})
				.catch(function(err) {
					if(err.response) {
						console.log(err.response)
					}
				})
			}
		}
	}
</script>
<style>
	body {
		background: #DFE9FB;
	}
</style>
<style lang="scss" scoped>
	.login-container {
		/*box-shadow: 0 0px 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0px 0 rgba(0, 0, 0, 0.02);*/
		-webkit-border-radius: 5px;
		border-radius: 5px;
		-moz-border-radius: 5px;
		background-clip: padding-box;
		margin: 160px auto;
		width: 350px;
		padding: 35px 35px 15px 35px;
		background: #fff;
		border: 1px solid #eaeaea;
		box-shadow: 0 0 25px #cac6c6;
		background: -ms-linear-gradient(top, #ace, #00C1DE);
		/* IE 10 */
		background: -moz-linear-gradient(top, #ace, #00C1DE);
		/*火狐*/
		background: -webkit-gradient(linear, 0% 0%, 0% 100%, from(#ace), to(#00C1DE));
		/*谷歌*/
		background: -webkit-linear-gradient(top, #ace, #00C1DE);
		/*Safari5.1 Chrome 10+*/
		background: -o-linear-gradient(top, #ace, #00C1DE);
		/*Opera 11.10+*/
		.title {
			margin: 0px auto 40px auto;
			text-align: center;
			color: #505458;
		}
		.remember {
			margin: 0px 0px 35px 0px;
		}
	}
</style>