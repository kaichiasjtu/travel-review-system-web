<template>
  <div class="registerContainer">
    <img src="../assets/login/login.png" class="top"></img>
    <p class="topmsg">注册</p>
    <form novalidate @submit.stop.prevent="submit">
    <md-input-container>
        <label>账号: </label>
        <md-input v-model="username"></md-input>
    </md-input-container>
    <md-input-container>
        <label>名字: </label>
        <md-input v-model="name"></md-input>
    </md-input-container>
    <md-input-container>
        <label>密码: </label>
        <md-input type="password" v-model="pwd"></md-input>
    </md-input-container>
    <md-input-container>
        <label>再输入一次密码: </label>
        <md-input type="password" v-model="pwdrepeat"></md-input>
    </md-input-container>
    </form>
    <md-button class="md-raised md-primary registerbtn" @click="register">注册</md-button>
    <snackbar ref="snackbar"></snackbar>
    <loading ref="loading"></loading>
  </div>
</template>

<script>
import snackbar from '@/components/SnackBar';
import { mapMutations } from 'vuex';
import { userRegister } from '@/service';
import loading from '@/components/Loading';

export default {
  data() {
    return {
      email: null,
      pwd: null,
      token: null,
      name: null,
      phone: null,
      companyid: null,
      companyName: null,
      roleid: null,
      userInfo: null,
    };
  },
  components: {
    snackbar,
    loading,
  },
  methods: {
    ...mapMutations([
      'RECORD_USERINFO',
    ]),
    register() {
      if (this.username == null || this.pwd == null
      || this.name == null || this.pwdrepeat == null) {
        this.$refs.snackbar.msg = '资料请填全！';
        this.$refs.snackbar.open();
        return;
      } else if (this.pwd !== this.pwdrepeat) {
        this.$refs.snackbar.msg = '密码不一致！';
        this.$refs.snackbar.open();
        return;
      }
      this.$refs.loading.open();
      userRegister({
        name: this.name,
        username: this.username,
        pwd: this.pwd,
        role: 3,
      }).then((success) => {
        if (success !== null && success.id != null) {
          this.$refs.snackbar.msg = '注册成功！';
          this.$refs.snackbar.open();
          this.RECORD_USERINFO({
            id: success.id,
            name: this.name,
            username: this.username,
            role: 3,
          });
          this.$refs.loading.close();
          this.$emit('closeDialog', 'accepted');
        } else {
          this.$refs.snackbar.msg = '注册失败！';
          this.$refs.snackbar.open();
          this.$refs.loading.close();
        }
      }, (error) => {
        /* eslint no-console: ["error", { allow: ["debug"] }] */
        console.debug(error);
      });
    },
  },
};
</script>

<style>
.registerContainer {
    width: 100%;
}

.registerbtn {
    font-size: 13pt;
    display: block;
    margin: 0 auto;
}

.top {
    width: 30%;
    display: block;
    margin: 0 auto;
    padding-bottom: 10%;
    padding-top: 5%;
}

.topmsg {
    font-size: 18pt;
    font-weight: 500;
    text-align:center;
    padding-bottom: 5%;
}
</style>
