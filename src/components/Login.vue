<template>
  <div class="login_container">
    <div class="login_box">
<!--      头像区域-->
      <div class="avatar_box">
        <img src="../assets/dog.jpg" alt="">
      </div>
<!--      登录表单区域-->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
<!--        用户名-->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
<!--        密码-->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
<!--        按钮区域-->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是表单的验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm() {
      // console.log(this)
      this.$refs.loginFormRef.resetFields()
    },

    login() {
      this.$refs.loginFormRef.validate(async valid => {
        // console.log(valid)
        // eslint-disable-next-line no-useless-return
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登陆失败！')
        this.$message.success('登陆成功！')
        // 1.将登录成功之后的token，保存到客户端的sessionStorage中
        //  1.1项目中除了登录之外的API接口，必须在登录之后才能访问
        //  1.2token只应当在当前网站打开期间生效，所以将token保存在sessionStrong中
        console.log(res)
        window.sessionStorage.setItem('token', res.data.token)
        // 2.通过编程式导航跳转到后台首页，路由地址是/home
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .login_container {
    /*background-color: rgb(183, 181, 210);*/
    background: -webkit-linear-gradient(left top, #3d3d3d, #fff); /* Safari 5.1 - 6.0 */
    background: -o-linear-gradient(bottom right, #3d3d3d, #fff); /* Opera 11.1 - 12.0 */
    background: -moz-linear-gradient(bottom right, #3d3d3d, #fff); /* Firefox 3.6 - 15 */
    background: linear-gradient(to bottom right, #3d3d3d, #fff); /* 标准的语法 */
    height: 100%;
  }

  .login_box {
    width: 450px;
    height: 300px;
    background-color: #ffffff;
    border-radius: 10px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 6px 3px #ccc;

    .avatar_box {
      height: 130px;
      width: 130px;
      border: 1px solid #eeeeee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #dddddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #ffffff;
      img{
        height: 100%;
        width: 100%;
        border-radius: 50%;
        background-color: #eeeeee;
      }
    }

    .login_form {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 0 20px;
      /*border-box 告诉浏览器去理解你设置的边框和内边距的值是包含在width内的。
      也就是说，如果你将一个元素的width设为100px,那么这100px会包含其它的border和padding，
      内容区的实际宽度会是width减去border + padding的计算值。*/
      box-sizing: border-box;
    }

    .btns {
      display: flex;
      justify-content: flex-end;
    }

  }
</style>
