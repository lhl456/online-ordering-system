<template>
  <div style="margin: 0 auto;">

    <div style="width: 90%;margin: 0 auto;margin-top: 20px">
      <el-space direction="vertical" alignment="left" size="large">

        <div style="text-align: center">
          <el-space>
            <el-input v-model="searchName" style="width: 500px" placeholder="吃点啥？" size="large"/>
            <el-button type="primary" icon="Search" @click="search"
                       style="background-color: RGB(255,15,35);border-color: #909399" size="large"><p style="font-size: 20px">搜索</p>
            </el-button>
          </el-space>
        </div>

        <el-row :gutter="10" style="height: 380px">

          <el-col :span="16">
            <el-carousel indicator-position="outside" height=" 380px">
              <el-carousel-item v-for="item in slideshow" :key="item" @click="openLink(item.link)">
                <el-image style=" width: 100%; height: 380px" fit="fill"
                          :src="item.mainImg"
                />
              </el-carousel-item>
            </el-carousel>
          </el-col>
          <el-col :span="4">
            <el-image style=" width: 100%; height: 380px" fit="fill"
                      @click="openLink(leftAdvertising.link)"
                      :src="leftAdvertising.mainImg"
            />
          </el-col>
          <el-col :span="4">
            <el-image style=" width: 100%; height: 380px" fit="fill"
                      @click="openLink(rightAdvertising.link)"
                      :src="rightAdvertising.mainImg"
            />
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="24">
            <el-card>
              <template #header>
                热销商品
              </template>
              <el-row :gutter="20" style="width: 100%">
                <el-col :span="4" v-for="(item,index) in productSalesVolumeTop"
                        :style="index>5 ? { marginTop: '20px',cursor: 'pointer' } : 'cursor: pointer'">
                  <Product :product="item"></Product>
                </el-col>
              </el-row>
            </el-card>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="24">
            <el-card>
              <template #header>
                猜你喜欢
              </template>
              <el-row :gutter="20" style="width: 100%">
                <el-col :span="4" v-for="(item,index) in productRecommend"
                        :style="index>5 ? { marginTop: '20px',cursor: 'pointer' } : 'cursor: pointer'">
                  <Product :product="item"></Product>
                </el-col>
              </el-row>
            </el-card>
          </el-col>
        </el-row>
      </el-space>
    </div>
  </div>
</template>
<script setup>
import {ref, toRaw} from "vue";
import request from "@/utils/http.js";
import Product from "@/components/Product.vue";
import router from "@/router/index.js";


const slideshow = ref([])
getSlideshowList()


function getSlideshowList() {
  request.get("/slideshow/page", {
    params: {pageNum: 1, pageSize: 10}
  }).then(res => {
    slideshow.value = res.data.list
  })
}

const productSalesVolumeTop = ref([])
getProductSalesVolumeTopList()

function getProductSalesVolumeTopList() {
  request.get("/product/salesVolumeTop/12").then(res => {
    productSalesVolumeTop.value = res.data
  })
}

const searchName = ref('')

function search() {
  openLink('/productList?name=' + searchName.value)
}

const leftAdvertising = ref({})
const rightAdvertising = ref({})


const advertisingList = ref([])
getAdvertisingList()

function getAdvertisingList() {
  request.get("/advertising/list").then(res => {
    advertisingList.value = res.data
    leftAdvertising.value = res.data.find(item => item.position === '轮播图左侧')
    rightAdvertising.value = res.data.find(item => item.position === '轮播图右侧')

  })
}

function openLink(link) {
  window.location.href = link
}

const productRecommend = ref([])
getProductRecommendList()

function getProductRecommendList() {
  request.get("/product/recommend/6").then(res => {
    productRecommend.value = res.data
  })
}

</script>

<style scoped>


.slideshow-card .el-card {
  --el-card-padding: 0;
}

.custom-input {
  border: none; /* 去掉边框 */
  border-bottom: 2px solid orange; /* 设置底部边框颜色为橙色 */
  border-radius: 0; /* 去掉圆角 */
  box-shadow: none; /* 去掉阴影 */
}

.custom-input .el-input__inner {
  background-color: transparent; /* 设置背景为透明 */
  padding: 10px; /* 调整内边距 */
  color: #333; /* 设置字体颜色 */
}

.custom-input .el-input__inner:focus {
  outline: none; /* 去掉聚焦时的轮廓 */
  border-bottom: 2px solid orange; /* 聚焦时保持橙色边框 */
}
</style>
