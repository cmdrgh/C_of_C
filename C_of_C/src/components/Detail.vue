<template>
  <div class="main">
    <header class="head">
      <img class="reback" src="../assets/reback.png" @click="reback">
      详情
    </header>
    <div class="info-table">
      <div class="form-div">
        <img class="icon" src="../assets/user.png">
        <div class="input-div">
          <span class="info">{{username}}</span>
        </div>
      </div>
      <div class="form-div">
        <img class="icon" src="../assets/phone.png">
        <div class="input-div">
          <span class="info">{{cellphone}}</span>
        </div>
      </div>
      <div class="form-div">
        <img class="icon" src="../assets/QQ.png">
        <div class="input-div">
          <span class="info info-null" v-show="QQinfo">QQ</span>
          <span class="info">{{qqnumber}}</span>
        </div>
      </div>
      <div class="form-div">
        <img class="icon" src="../assets/wx.png">
        <div class="input-div">
          <span class="info info-null" v-show="wxinfo">微信</span>
          <span class="info">{{wechat}}</span>
        </div>
      </div>
      <div class="form-div">
        <img class="icon" src="../assets/occupation.png">
        <div class="input-div spacial">
          职业：<span class="info">{{occupation}}</span>
        </div>
      </div>
      <div class="form-div">
        <img class="icon" src="../assets/location.png">
        <div class="input-div spacial">
          原籍：<span class="info">{{town}}</span>
          <span class="info" style="margin-left: 0.6rem;">{{village}}村</span>
        </div>
      </div>
      <div class="form-div">
        <img class="icon" src="../assets/team.png">
        <div class="input-div">
          <span class="info info-null" v-show="teaminfo">工作单位</span>
          <span class="info">{{team}}</span>
        </div>
      </div>
      <div class="form-div">
        <img class="icon" src="../assets/address.png">
        <div class="input-div">
          <span class="info info-null" v-show="addressinfo">地址</span>
          <span class="info">{{address}}</span>
        </div>
      </div>
    </div>
    <Navigation :tagid="1"></Navigation>
  </div>
</template>

<script>
import Navigation from './Navigation.vue'

export default {
  name: 'Detail',
  data () {
    return {
      username: '',
      cellphone: '',
      qqnumber: '',
      wechat: '',
      occupation: '',
      town: '',
      village: '',
      team: '',
      address: '',
      QQinfo: false,
      wxinfo: false,
      teaminfo: false,
      addressinfo: false,

      pagenum: ''
    }
  },
  components: {
    Navigation
  },
  created() {
    var param = this.$route.params.userId;
    var _this = this;

    this.pagenum = this.$route.params.curPage;

    // console.log(param);
    this.axios({
      url: this.baseUrl + '/user/info/detail?userId=' + param,
      // url: '/api/user/info/detail?userId=' + param,
      method: 'get',
      headers: {
        "S-TOKEN": this.$cookies.get('token')
      }
    })
    .then(function(res) {
      // console.log(res);
      _this.username = res.data.data.userInfoName;
      _this.cellphone = res.data.data.userInfoPhone;
      _this.occupation = res.data.data.userInfoOccupation;
      _this.town = res.data.data.userInfoTown;
      _this.village = res.data.data.userInfoVillage;

      _this.qqnumber = res.data.data.userInfoQQ;
      _this.wechat = res.data.data.userInfoWechat;
      _this.team = res.data.data.userInfoUnit;
      _this.address = res.data.data.userInfoAddress;

      if(_this.qqnumber === '') {
        _this.QQinfo = true;
      }
      if(_this.wechat === '') {
        _this.wxinfo = true;
      }
      if(_this.team === '') {
        _this.teaminfo = true;
      }
      if(_this.address === '') {
        _this.addressinfo = true;
      }
    })
    .catch(function(error) {
      console.log(error);
      _this.$toast('信息读取失败');
    })
  },
  methods: {
    reback: function() {
      this.$router.push({
        name: "AddrList",
        params: {
          pageNum: this.pagenum
        }
      });
    }
  }
}
</script>

<style scoped>
.main {
  width: 100%;
  height: 100%;
  font-size: 0.52rem;
  /* background-color: #f4f4f4; */
  background-color: #fff;
}

.head {
  position: relative;
  width: 100%;
  padding: 0.28rem 0;
  border-bottom: 1px solid #ebebeb;
  color: #f39839;
  background-color: #fff;
}

.reback {
  position: absolute;
  top: 0.42rem;
  left: 0.36rem;
  width: 0.5rem;
}

.info-table {
  padding: 0.62rem;
  background-color: #fff;
}

.form-div {
  position: relative;
  line-height: 0.9rem;
  margin-bottom: 0.6rem;
  text-align: left;
  font-size: 0.58rem;
}

.icon {
  width: 0.9rem;
  margin-bottom: -0.3rem;
}

.input-div {
  position: absolute;
  top: 0;
  left: 1.4rem;
  width: 82%;
  height: 0.98rem;
  line-height: 0.98rem;
  color: #303133;
  border-bottom: 0.01rem solid #ebebeb;
}

.info {
  display: inline-block;
  margin-left: 0.14rem;
}

.info-null {
  color: #909399;
}
</style>
