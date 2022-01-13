<template>
  <div>
    <!-- 表单 -->
    <el-form
      label-width="0px"
      class="login_form"
      :model="loginform"
      :rules="loginrules"
      ref="loginref"
    >
      <div class="title">
        <div class="title-text">用户登录</div>
      </div>

      <!-- 用户名 -->
      <el-form-item prop="username">
        <el-input
          v-model="loginform.username"
          placeholder="用户名或者电子邮箱"
          prefix-icon="el-icon-user"
        ></el-input>
      </el-form-item>

      <!-- 密码 -->
      <el-form-item prop="passward">
        <el-input
          placeholder="密码"
          v-model="loginform.password"
          show-password
          maxlength="16"
          minlength="3"
          prefix-icon="el-icon-lock"
        >
        </el-input>
      </el-form-item>

      <!--按钮 -->
      <el-form-item class="selectbtns">
        <el-radio v-model="loginform.radio" label="1">管理员</el-radio>
        <el-radio v-model="loginform.radio" label="3">学生</el-radio>
      </el-form-item>

      <el-form-item class="touchbtns">
        <el-button type="primary" @click="login">登录</el-button>
        <el-button type="info" @click="register">注册</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    return {
      loginform: { username: "", password: "", radio: "", },
      //校验规则
      radio: "1",
      loginrules: {
        username: [
          { required: true, message: "请输入登录名称", trigger: "blur" },
          {
            min: 3,
            max: 10,
            message: "长度在 3 到 10 个字符",
            trigger: "blur",
          },
        ],
        password: [
          { required: true, message: "请输入登录密码", trigger: "blur" },
          {
            min: 3,
            max: 16,
            message: "长度在 3 到 16 个字符",
            trigger: "blur",
          },
        ],
      },
    };
  },
  components: {},
  //生命周期 - 创建完成（访问当前this实例）
  created() {
    window.sessionStorage.clear();
  },
  //生命周期 - 挂载完成（访问DOM元素）
  mounted() {},
  methods: {
    register() {
      this.$router.push("/register");
    },
    login() {
      this.$http
        .post("user/login", this.$qs.stringify(this.loginform))
        .then((res) => {
          if (res.data.meta.status !== 200)
            return this.$message.warning("登陆失败");
          this.$store.commit("set_token", res.data.data[0].token);
          this.$store.commit("set_radio", res.data.data[0].radio);
          //设置token
          window.sessionStorage.setItem("radio", res.data.data[0].radio);
          
          if (this.$store.state) {
            this.$message.success("登陆成功");
            this.$router.push("/home");
          } else {
            this.$router.push("/login");
          }
        })
        .catch((err) => {
          this.$message.error("请求连接错误");
        });
    },
  },
};
</script>
<style lang="less" scoped>
/* @import url(); 引入css类 */
.title {
  position: relative;
  margin-bottom: 20px;
  text-align:center;
  .title-text {
    font-size: 24px;
    font-weight: 400;
    color: #4876e9;
  }
  .title-eng {
    margin-top: 9px;
    font-size: 14px;
    font-weight: 400;
    color: #7a7575;
  }
}

.top {
  text-align: center;
  padding: 16px 16px 0px 16px;
  border-bottom: 2px solid rgba(100, 100, 100, 0.1);
  background-color: #51b7ec;
  img {
    width: 60px;
    height: 60px;
    border: 1px solid blue;
  }
}
.head h1 {
  margin-block-end: 15px;
  text-align: center;
  line-height: 25.4px;
  color: rgba(0, 0, 0, 0.56);
  // margin-block-start: 250px;
}
.login_form {
  width: 480px;
  height: 300px;
  // background-color: rgb(250, 250, 250);
  box-shadow: 10px 10px 10px 10px rgba(0, 0, 0, 0.15) !important;
  position: absolute;
  left: 50%;
  top: 50%;
  padding: 30px 10px 0px 20px;
  transform: translate(-50%, -50%);
  background-color: rgba(250, 250, 250, 0.55);
  .el-input {
    margin-bottom: 10px;
  }
}
.selectbtns {
  align-content: center;
  position: absolute;
  top: 65%;
  width: 300px;
  display: flex;
  justify-content: flex-end;
}

.touchbtns {
  bottom: 0;
  left: 36%;
  position: absolute;
  align-content: center;
  top: 80%;
}
</style>