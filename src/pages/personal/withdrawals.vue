<template>
  <div class="withgrawals">
    <div class="commonNavBar positionFixed">
      <div class="backPassTitle"><p>提现密码</p></div>
      <div class="loginIcon arrowLeft flt" @click="$router.go(-1)"><a href="javascript:void(0)"></a></div>
    </div>
    <div class="withgrawalsContentBox">
      <div class="withgrawalsContent">
        <div class="loginLi displayFlex">
          <p>设置密码</p>
          <p>
            <input type="password" v-model="pwdOne" placeholder="请输入6～12位数字或字母" maxlength="12"/>
          </p>
        </div>
        <div class="loginLi displayFlex">
          <p>确认密码</p>
          <p>
            <input type="password" v-model="pwdTwo" placeholder="请再次输入您的密码" maxlength="12"/>
          </p>
        </div>
        <div @click="save()" class="loginHostBtn finshBtn"><a href="javascript:void(0);">完成</a></div>
      </div>
      
    </div>
  </div>
</template>
<script>
  
  export default {
    data() {
      return {
        pwdOne: '',
        pwdTwo: ''
      }
    },
    mounted() {

    },
    methods: {
      save() {
        let that = this;
        
        let reg = /^[0-9a-zA-z]{6,12}$/;
        
        if (!that.pwdOne) {
          that._Util.showAlert(that, {content: '密码不能为空'});
          return;
        }
        
        if (!reg.test(that.pwdOne)) {
          that._Util.showAlert(that, {content: '[密码]请输入6～12位数字及字母'});
          return;
        }
        
        if (that.pwdOne !== that.pwdTwo) {
          that._Util.showAlert(that, {content: '输入的密码不一致'});
          return;
        }
        
        that._Util.post(that, that._Api.POST_PERSONAL_MEMBER_BANK_UPD_PWD, {
          phone: that.$route.query.phone,
          code: that.$route.query.code,
          password: that._Util.hexMd5(that.pwdOne)
        }, (data) => {
          that._Util.showAlert(that, {content: "修改成功"}, () => {
            if (that.$route.query.type == 2) {
              that.$router.replace({name: 'cash'});
            } else {
              that.$router.replace({path: '/home?homeIndex=3'});
            }
          })
        });
      }
    }
  }
</script>

<style lang='scss'>
  /*body, html {*/
  /*height: 100%;*/
  /*overflow-y: scroll;*/
  /*}*/
  /*#app{*/
  /*height:100%;*/
  /*}*/
  /*.view{*/
  /*height: 100%;*/
  /*}*/
</style>
