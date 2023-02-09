<script setup>
import {onMounted, reactive, ref} from "vue"
import { User, Lock, Message } from '@element-plus/icons-vue'
import request from "@/utils/request";
import {ElMessage} from "element-plus";
import { useUserStore } from "@/stores/user";
import router, {setRoutes} from "@/router";

  const loginData = reactive({})
  const rules = reactive({
    username: [
      { required: true, message: '请输入账号', trigger: 'blur' },
    ],
    email: [
      { required: true, message: '请输入邮箱', trigger: 'blur' },
    ],
    password: [
      { required: true, message: '请输入密码', trigger: 'blur' },
      { min: 3, max: 20, message: '密码长度在3-20位之间', trigger: 'blur' },
    ],
    confirmPassword: [
      { required: true, message: '请确认密码', trigger: 'blur' },
      { min: 3, max: 20, message: '密码长度在3-20位之间', trigger: 'blur' },
    ],
  })
  const ruleFormRef = ref()
  const login = () => {
    ruleFormRef.value.validate(valid => {
      if (valid) {
        if (loginData.password !== loginData.confirmPassword) {
          ElMessage.warning('两次密码不一致')
        }
        // 发送表单数据给后台
        request.post('/register', loginData).then(res => {
          if (res.code === '200') {
            ElMessage.success('注册成功')
            router.push('/login')
          } else {
            ElMessage.error(res.msg)
          }
        })
      }
    })
  }

</script>

<template>
  <div style="height: 100vh; overflow: hidden; background-color: aliceblue">
    <div style="width: 400px; margin: 150px auto; background-color: white; border: 1px solid #ddd; padding: 30px; border-radius: 10px">
      <el-form
          ref="ruleFormRef"
          :model="loginData"
          :rules="rules"
          size="large"
          status-icon
      >
        <div style="text-align: center; color: dodgerblue; font-size: 30px; font-weight: bold; margin-bottom: 30px">注 册</div>
        <el-form-item prop="username">
          <el-input v-model="loginData.username" placeholder="请输入账号"  :prefix-icon="User" />
        </el-form-item>
        <el-form-item prop="email">
          <el-input v-model="loginData.email" placeholder="请输入邮箱"  :prefix-icon="Message" />
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="loginData.password" show-password placeholder="请输入密码" :prefix-icon="Lock" />
        </el-form-item>
        <el-form-item prop="confirmPassword">
          <el-input v-model="loginData.confirmPassword" show-password placeholder="请确认密码" :prefix-icon="Lock" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" style="width: 100%" @click="login">注 册</el-button>
        </el-form-item>
        <div style="text-align: right"><a style="text-decoration: none; color: dodgerblue" href="/login">已有账号？去登录</a></div>
      </el-form>
    </div>
  </div>
</template>
