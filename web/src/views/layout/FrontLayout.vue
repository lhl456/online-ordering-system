<template>
  <div class="main-context">
    <el-container class="my-container">
      <el-header class="my-header">
        <el-row class="nav">
          <el-col :span="4" class="logo">
            <div style="text-align: right;padding-top: 10px;margin-right: 20px" @click="router.push('/')">
              <el-space>
                <img src="@/assets/logo.jpg" width="100%" style="width: 40px">
                <el-space direction="vertical" style="width: 100%" size="small">
                  <h3 style="font-style: oblique;color: white">在线订餐系统</h3>
                </el-space>
              </el-space>
            </div>
          </el-col>
          <el-col :span="16">
            <div style="text-align: center">
              <el-menu
                  :default-active="useRoute().path"
                  mode="horizontal"
                  router
                  text-color="white"
                  active-text-color="white"
                  background-color="rgba(0, 0, 0, 0)"
                  @select="handleSelect"
              >
                <el-menu-item index="/index">首页</el-menu-item>
                <el-menu-item index="/productList">商品列表</el-menu-item>
                <el-menu-item index="/shoppingCart">购物车</el-menu-item>
                <el-menu-item index="/productOrder">我的订单</el-menu-item>
                <el-menu-item index="/personalCenter">个人中心</el-menu-item>
              </el-menu>
            </div>
          </el-col>
          <el-col :span="4">
            <div style="text-align: center;">
              <el-space style="margin-top: 15px;">
                <el-dropdown v-if="isUserLogin">
                  <div>
                    <el-space>
                      <el-avatar style="width: 30px;height: 30px;border-radius: 50%"
                                 shape="square" :size="30" :src="currentUser.avatarUrl"></el-avatar>
                      <span style="font-size: 16px;color: white">  {{ currentUser.username }}</span></el-space>
                  </div>
                  <template #dropdown>
                    <el-dropdown-menu>
                      <el-dropdown-item><span @click="editCurrentUser">个人信息</span></el-dropdown-item>
                      <el-dropdown-item><span @click="editPassword">修改密码</span></el-dropdown-item>
                      <el-dropdown-item><span @click="balanceInfo">余额/充值</span></el-dropdown-item>
                      <el-dropdown-item divided><span @click="logout">退出登录</span></el-dropdown-item>
                    </el-dropdown-menu>
                  </template>
                </el-dropdown>
              </el-space>
            </div>
          </el-col>
        </el-row>
      </el-header>
      <el-main class="my-main">
        <router-view/>
      </el-main>
	</el-container>
  </div>
</template>

<script setup>
import tools from "@/utils/tools.js";
import {ref} from "vue";
import router from "@/router/index.js";
import {ElMessage} from "element-plus";
import {useRoute} from "vue-router";

const isUserLogin = ref(tools.isLogin())
const currentUser = ref(tools.getCurrentUser())

if (currentUser.value === null) {
  window.location.href = "/login"
}
if (currentUser.value && currentUser.value.type !== 'USER') {
  router.push({path: "/admin"})
}

function handleSelect(key, keyPath) {
  console.log(key, keyPath);
}

function logout() {
  ElMessage({
    message: '退出登录成功，正在跳转',
    type: 'success'
  });
  localStorage.clear()
  router.push({path: "/login"})
}

function editCurrentUser() {
  router.push({path: "/editCurrentUser"})
}

function editPassword() {
  router.push({path: "/editPassword"})
}

function balanceInfo() {
  router.push({path: "/balanceInfo"})
}

</script>


<style scoped>

.main-context {
  background: url("@/assets/bg2.jpg") no-repeat center center fixed !important;
  background-size: cover; /* 使图片宽度占满 */
  height: 50vh; /* 设定容器高度 */
}

.my-container {
  display: flex;
  flex-direction: column;
  height: 100vh; /* 例如，设置为视窗的高度 */
}

/*.my-header {*/
/*  background-color: rgba(255, 120, 0, 0.1);*/
/*}*/

.el-menu.el-menu--horizontal {
  border-bottom: none;
}

/*.my-main {*/
/*  background: linear-gradient(to bottom, rgba(255, 209, 0, 0.8) 0%, rgba(255, 255, 0, 0) 100%);*/
/*  height: 100vh; !* 设定容器高度 *!*/
/*}*/

.my-main::-webkit-scrollbar {
  display: none; /* 隐藏滚动条 */
}

.my-footer {
  font-size: 14px;
  padding: 10px;
  color: #999;
  background-color: white;
  text-align: center;
}
</style>
