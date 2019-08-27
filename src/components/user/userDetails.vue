<template>
  <el-row class="warp">
    <el-col :span="24" class="warp-breadcrum" :loading="loading">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }"><b>首页</b></el-breadcrumb-item>
        <el-breadcrumb-item>用户</el-breadcrumb-item>
        <el-breadcrumb-item>用户详情</el-breadcrumb-item>
      </el-breadcrumb>
    </el-col>
    
     <div>
    	this is the news page.the transform param is {{this.$route.params.UserDetails.userName}},{{user.userId}}
    	
  		</div>
    
		
    <el-col :span="24" class="warp-main">
      <el-form ref="form" :model="form" :rules="rules" label-width="80px">
        <el-form-item label="账号">
          <el-input v-model="form.userName" disabled></el-input>
        </el-form-item>
        <el-form-item prop="companyName" label="公司">
          <el-input v-model="form.companyName" disabled></el-input>
        </el-form-item>
        <el-form-item prop="userLevel" label="等级">
          <el-input v-model="form.userLevel" disabled></el-input>
        </el-form-item>
        <el-form-item>
        <!--  <el-button type="primary" @click="handleSaveProfile">修改并保存</el-button>-->
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>

<script>
 	import Global from '../../components/Global';
	import axios from 'axios';

  export default {
    data() {
      return {
        loading: false,
        form: {
          userName: '',
          companyName: '',
          userLevel: '',
        },
        user : this.$route.params.UserDetails,
        rules: {
          nickname: [
            {required: true, message: '请输入昵称', trigger: 'blur'}
          ],
        },
      }
    },
    methods: {
      handleSaveProfile() {
      },
    },
    mounted() {
        this.form.userName = localStorage.getItem('userName') || '';
        this.form.companyName = localStorage.getItem('companyName')|| '';
        this.form.userLevel = localStorage.getItem('userLevel') || '';
    }
  }
</script>
