<template>
    <div class="application">
        <div v-title>历史记录</div>
        <div v-if="dataList.length > 0" v-infinite-scroll="getHireOffic" infinite-scroll-disabled="loading"
      infinite-scroll-distance="10">
          <div class="hire-detail" v-for="(item, index) in dataList" :key="index" @click="jumpToDetail(item.officeId)">

            <div class="bo-offic-box">
              <div class="bo-offic-name ellipsis-1">
                {{item.offName}}
              </div>
              <div class="bo-offic-money ellipsis-1">
                {{item.offMoney}}
              </div>
            </div>
            <div class="bo-offic-msg ellipsis-1">
              {{item.offPlace}}&#x3000;{{item.offEducation}}&#x3000;{{item.offExperience}}
            </div>
            <div class="bo-company-box">
              <div class="bo-company-img">
                <img :src="item.company.companyImg" alt="">
              </div>
              <div class="bo-company-msg">
                <div class="bo-company-name ellipsis-1">
                  {{item.company.companyName}}
                </div>
                <div class="bo-company-fu ellipsis-1">
                  {{item.company.companyDet}}&#x3000;{{item.company.companyPer}}&#x3000;{{item.company.companyType}}
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-else class="no-msg">
            <img src="../../static/no-msg.png" alt="">
        </div>
    </div>
</template>
<script>
import moment from 'moment'
import { MessageBox } from 'mint-ui'
export default {
    name: 'History',
    data() {
      return {
        dataList: [],
        userId: '',
        hirePage: 0
      }
    },
    filters: {
      time: (value) => {
        return moment(value).format('YYYY-MM-DD')
      }
    },
    created() {

      this.userId = window.localStorage.getItem('userMsg') && JSON.parse(window.localStorage.getItem('userMsg')).id
      if (!this.userId) {
        this.$router.back(-1)
      }
      this.getHireOffic()
    },
    methods: {
      getHireOffic: function () {
        this.hirePage++
        this.axios({
          method: 'post',
          url: '/api/getHistoryList',
          headers: {
            'Content-type': 'application/json;charset=UTF-8'
          },
          data: {
            page: this.hirePage,
            userId: this.userId
          }
        }).then((res) => {
          if (res.data.code == '200' || res.data.code == 200) {
            let data = res.data.data
            for (let i = 0; i < data.length; i++) {
              if (!data[i].company) {
                data[i].company = {
                  companyImg: '../../static/ge-zi-icon.png',
                  companyName: '默认公司',
                  companyDet: '未知',
                  companyPer: '未知',
                  companyType: '未知'
                }
              }
            }
            this.dataList = this.dataList.concat(data)
          }
        })
      },
      jumpToDetail: function (oid) {
        window.sessionStorage.setItem('office', oid)
        this.$router.push({
          path: '/office'
        })
      }
    }
}
</script>
<style scoped>
.no-msg {
  height: 100vh;
  text-align: center;
  padding-top: 1px;
  background-color: #fff;
}
.no-msg img{
  width: 50%;
  margin-top: 40%;
}
.jot-applicat-card {
    background-color: #fff;
    padding: 10px;
    margin-bottom: 10px;
    overflow: hidden;
}
.jot-applicat-card-left {
    float: left;
    width: 70%；
}
.jot-applicat-card-right {
    float: right;
    height: 30%;
}
.job-title {
    margin-bottom: 10px;
}
.hire-detail {
  overflow: hidden;
  box-sizing: border-box;
  width: 100%;
  padding: 10px;
  background-color: #fff;
  margin-bottom: 10px;
  border-bottom: 1px solid rgb(197, 196, 196);
}
.hire-detail-left {
  float: left;
  width: 60px;
  height: 60px;
  margin-right: 20px;
}
.hire-detail-left img {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  border: 1px solid rgb(197, 196, 196);
}
.company-off-card-title {
  overflow: hidden;
  padding-bottom: 5px;
}
.company-off-card-title-left {
  float: left;
  font-size: 16px;
  max-width: 70%;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-weight: 500;
}
.company-off-card-title-right {
  float: right;
  max-width: 30%;
  white-space: nowrap;
  text-overflow: ellipsis;
  text-align: right;
  font-size: 17px;
  color: #5dd5ca;
  margin-right: 30px
}

.hire-detail-right {
  float: left;
  width: calc(100% - 80px)
}
</style>
