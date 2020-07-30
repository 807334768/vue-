<template>
	<div class="loginbg">
		<el-form ref="form" :model="form" :rules="loginRules" class="form" autocomplete="on" label-position="left">
			<div class="loginMain">
				<el-form-item prop="code">
					<img src="../../assets/imgs/code.png" class="icon">
					<el-input ref="code" v-model="form.code" type="text" placeholder="验证码" name="code"  
					 autocomplete="on" @keyup.enter.native="handleLogin" class="inputStyle" />
					<div class="code" @click="refreshCode">
						<s-identify :fresh="flag" @makedCode="getMakedCode"></s-identify>
					</div>
				</el-form-item>
				<el-button class="loginBtn loginBtnRight" @click.native.prevent="handleLogin">登录</el-button>
			</div>
	</div>

	</el-form>
	</div>
</template>

<script>
	import SIdentify from './index.vue';
	export default {
		name: "Login",
		components: {
			SIdentify
		},
		data() {
			const validateCode = (rule, value, callback) => {
				if (value != this.code) {
					callback(new Error("验证码不正确"));
				} else {
					callback();
				}
			}
			return {
				form: {
					code: ''
				},
				loginRules: {
					code: [{
						required: true,
						trigger: "blur",
						validator: validateCode
					}]
				},
				flag: true, //该值变化，就会触发刷新
				code: '' //刷新后的验证码
			};
		},
		mounted() {
			this.flag = !this.flag;
		},

		methods: {
			refreshCode() {
				this.flag = !this.flag;
			},
			getMakedCode(code) {
				this.code = code;
				console.log('getMakedCode:', this.code);
			},


		}
	};
</script>
