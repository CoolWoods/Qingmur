<template>
  <div class="login-box">
    <el-row>
      <el-form :model="loginForm"
               :rules="rules"
               ref="loginForm"
               status-icon
               label-width="100px"
               class="loginForm">
        <el-form-item label=""
                      prop="mobilePhone">
          <el-input v-model="loginForm.mobilePhone"
                    placeholder="手机号码"
                    prefix-icon="el-icon-user">
          </el-input>
        </el-form-item>

        <el-form-item label=""
                      prop="password">
          <el-input v-model="loginForm.password"
                    type="password"
                    placeholder="密码"
                    prefix-icon="el-icon-lock">
          </el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary"
                     @click="onLogin">登录</el-button>
          <el-button type="primary"
                     @click="resetLoginForm('loginForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'Index',
  data () {
    var validateMobilePhone = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入手机号码'))
      } else {
        var reg = new RegExp('\\d')

        if (!reg.test(value)) {
          callback(new Error('请输入数字'))
        } else if (value.length !== 11) {
          callback(new Error('请输入11位手机号'))
        }
      }
      callback()
    }

    var validatePassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'))
      } else {
        if (value.length < 6) {
          callback(new Error('密码长度小于6'))
        } else if (value.length > 32) {
          callback(new Error('密码长度大于32'))
        }
        console.log(value)
        var reg1 = new RegExp('^(?=.*[0-9])(?=.*[a-zA-Z])(.{6,32})$')
        if (!reg1.test(value)) {
          callback(new Error('密码包含数字和字母'))
        } else {
          callback()
        }
      }
    }

    return {
      loginForm: {
        mobilePhone: '',
        password: ''
      },
      rules: {
        // username的rule使用自带的，而password的rule使用自定义的，效果相当，但是自定义的更加灵活
        mobilePhone: [
          { validator: validateMobilePhone, trigger: ['blur', 'change'] }
        ],
        password: [
          { validator: validatePassword, trigger: ['blur', 'change'] }
        ]
      }
    }
  },
  methods: {
    resetLoginForm (loginForm) {
      this.$refs[loginForm].resetFields()
    },
    onLogin () {
      console.log('login!')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login-box {
  width: 50%;
  height: 100%;
  max-width: 420px;
  margin-left: auto;
  margin-right: auto;
}
</style>
