<template>
  <div class="loginbg">
    <el-form
      ref="form"
      :model="form"
      :rules="loginRules"
      class="form"
      autocomplete="on"
      label-position="left"
    >
    <div class="loginMain">
    	<img src="../../assets/imgs/loginLogo.png" class="loginLogo pt" />
    	<div class="loginDiv ptFirst">
        <el-form-item prop="username">
          <img src="../../assets/imgs/user.png" class="icon">
          <el-input
            ref="username"
            v-model="form.username"
            placeholder="姓名/身份证/手机号"
            name="username"
            type="text"
            tabindex="1"
            autocomplete="on"
             class="inputStyle"
          />
        </el-form-item>

<el-form-item prop="password">
          <img src="../../assets/imgs/pwd.png" class="icon">
        <el-input
          :key="passwordType"
          ref="password"
          v-model="form.password"
          :type="passwordType"
          placeholder="密码"
          name="password"
          tabindex="2"
          autocomplete="on"
          @keyup.enter.native="handleLogin"
            class="inputStyle"
        />
        <span class="show-pwd eye" @click="showPwd">
          <svg-icon
            :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'"
          />
        </span>
      </el-form-item>
<el-form-item prop="code">
          <img src="../../assets/imgs/code.png" class="icon">
        <el-input
          
          ref="code"
          v-model="form.password"
          type="text"
          placeholder="验证码"
          name="password"
          tabindex="2"
          autocomplete="on"
          @keyup.enter.native="handleLogin"
            class="inputStyle"
        />
       <div class="code"
             @click="refreshCode">
          <s-identify :fresh="flag"
                      @makedCode="getMakedCode"></s-identify>
        </div>
      </el-form-item>




    		<div class="textCon">
    			<input type="checkbox" />
    			<span class="hinText">记住密码</span>
    		<!-- 	<span  class="hinText ml79">忘记密码</span> -->
    		</div>
    		<el-button class="loginBtn loginBtnLeft"   @click.native.prevent="handleLogin">人脸识别</el-button>
    		<el-button class="loginBtn loginBtnRight"   @click.native.prevent="handleLogin">登录</el-button>
    	</div>
    </div>
    <!--  <div class="title-container">
        <h3 class="title">
          登录
        </h3>
      </div>
 -->
      <!-- <el-form-item prop="username">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          ref="username"
          v-model="form.username"
          placeholder="用户名"
          name="username"
          type="text"
          tabindex="1"
          autocomplete="on"
        />
      </el-form-item> -->

    <!--  <el-form-item prop="password">
        <span class="svg-container">
          <svg-icon icon-class="lock" />
        </span>
        <el-input
          :key="passwordType"
          ref="password"
          v-model="form.password"
          :type="passwordType"
          placeholder="密码"
          name="password"
          tabindex="2"
          autocomplete="on"
          @keyup.enter.native="handleLogin"
        />
        <span class="show-pwd" @click="showPwd">
          <svg-icon
            :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'"
          />
        </span>
      </el-form-item> -->

      <!-- <el-row :gutter="24">
        <el-col :xs="24" :sm="16">
          <el-button
            :loading="loading"
            class="mainBtn cs-mb20"
            type="primary"
            @click.native.prevent="handleLogin"
          >
            登 录
          </el-button>
        </el-col>
        <el-col :xs="24" :sm="8">
          <router-link to="/register">
            <el-button class="subBtn cs-mb20">
              注 册
            </el-button>
          </router-link>
        </el-col>
      </el-row> -->
    </el-form>
  </div>
</template>

<script>
  import SIdentify from '../../components/code/index.vue';
export default {
  name: "Login",
   components: {
          SIdentify
      },
  data() {
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error("密码最少需要 6 位"));
      } else {
        callback();
      }
    };
    return {
      form: {
        // username: "admin",
        // password: "123455"
      },
      loginRules: {
        username: [
          { required: true, trigger: "blur", message: "请填写正确的用户名！" }
        ],
        password: [
          { required: true, trigger: "blur", validator: validatePassword }
        ]
      },
      loading: false,
      passwordType: "password",
      redirect: undefined,
       flag: true, //该值变化，就会触发刷新
      code: '' //刷新后的验证码
    };
  },
  components: {
          SIdentify
      },
      mounted() {
          this.flag = !this.flag;
      },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true
    }
  },
  methods: {
    refreshCode() {
                this.flag = !this.flag;
            },
            getMakedCode(code) {
                this.code = code;
                console.log('getMakedCode:', this.code);
            },
    showPwd() {
      if (this.passwordType === "password") {
        this.passwordType = "";
      } else {
        this.passwordType = "password";
      }
      this.$nextTick(() => {
        this.$refs.password.focus();
      });
    },
    handleLogin() {
      this.$refs.form.validate(valid => {
        if (valid) {
          this.loading = true;
          this.$store
            .dispatch("user/login", this.form)
            .then(res => {
              this.$router.push({ path: this.redirect || "/" });
              this.loading = false;
            })
            .catch(() => {
              this.loading = false;
            });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
@import "./index.scss";
</style>
