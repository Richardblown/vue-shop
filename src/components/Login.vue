<template>
    <div class="container">
      <div class="box">
        <!--图片区-->
        <div class="img_box">
            <img src="../assets/logo.png">
        </div>
        <!--表单区-->
        <el-form ref='loginRef' :model='form' label-width="0px" class="login" :rules='rules'>
            <el-form-item prop="username">
                <el-input v-model='form.username' prefix-icon="iconfont iconwode" placeholder="请输入用户名"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input v-model='form.password' prefix-icon="iconfont iconmima" type='password' placeholder="请输入密码"></el-input>
            </el-form-item>
            <!--按钮区域-->
            <el-form-item class="btn">
                <el-button type="primary" @click='validate'>登录</el-button>
                <el-button type="info" @click='reset'>重置</el-button>
            </el-form-item>
        </el-form>
      </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, min: 5, max: 15, message: '请输入正确的用户名，长度在5到15个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, min: 5, max: 15, message: '请输入正确的密码，长度在5到15个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    reset () {
      this.$refs.loginRef.resetFields()
    },
    validate () {
      this.$refs.loginRef.validate(async res => {
        if (!res) return
        const { data: result } = await this.$http.post('login', this.form)
        if (result.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        window.sessionStorage.setItem('token', result.data.token)
        this.$router.push('/Home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.container{
    background-color: #2b4b6b;
    height: 100%;
}
.box{
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
.img_box{
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 10px;
    background-color: #fff;
    img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
    }
}
.login{
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
}
.btn{
    display: flex;
    justify-content: flex-end;
}
</style>
