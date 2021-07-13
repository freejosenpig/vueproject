<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 账户登录表单区-->
      <el-tabs v-model="activeName" :stretch="true">
        <el-tab-pane label="账号登录" name="first">
          <div style="float: left; margin-top: 250px">
            <el-form
              ref="loginForm"
              :rules="loginFormRules"
              :model="loginForm"
              label-width="0px"
              class="login_form"
            >
              <!-- 用户名-->
              <el-form-item prop="userName">
                <el-input
                  prefix-icon="el-icon-user"
                  placeholder="请输入用户名"
                  maxlength="15"
                  v-model="loginForm.userName"
                  clearable
                ></el-input>
              </el-form-item>
              <!-- 密码-->
              <el-form-item prop="userPass">
                <el-input
                  prefix-icon="el-icon-lock"
                  placeholder="请输入密码"
                  v-model="loginForm.userPass"
                  maxlength="15"
                  show-password
                  clearable
                ></el-input>
              </el-form-item>
              <el-form-item class="btus">
                <!-- 确认登录-->
                <el-button type="primary" @click="login()" style="width: 415px"
                  >登录</el-button
                >
              </el-form-item>
            </el-form>
          </div>
        </el-tab-pane>

        
      </el-tabs>
    </div>
  </div>
</template>

<script>
import { initDynamicRoutes } from "../router/index.js";
import { ElMessage } from "element-plus";
export default {
  name: "Login",
  data() {
    return {
      activeName: "first",
      // 账户登录信息
      loginForm: {
        userName: "admin",
        userPass: "123456",
      },
      //表单验证规则对象
      loginFormRules: {
        //验证用户名是否合法
        userName: [
          { required: true, message: "请输入用户名", trigger: "blur" },
        ],
        //验证密码是否合法
        userPass: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "长度在 6 到 15 个字符",
            trigger: "blur",
          },
        ],
      },

      // 快捷登录信息
      loginFormgo: {
        userPhone: "",
        userMessage: "",
      },
      //表单验证规则对象
      loginFormRulesgo: {
        //验证用户名是否合法
        userPhone: [
          { required: true, message: "请输入手机号", trigger: "blur" },
        ],
        //验证密码是否合法
        userMessage: [
          { required: true, message: "请输入验证码", trigger: "blur" },
        ],
      },
      djs: 60,
      reset: false,
    };
  },
  methods: {
    //账户登录校验
    login() {
      const _this = this;
      this.axios
        .post("http://localhost:8188/sysUser/login", _this.loginForm)
        .then(function (response) {
          if (typeof response.data.data == "string") {
            ElMessage.error(response.data.data);
          } else {
            console.log(response)
            _this.$store.commit("setmenulists", response.data.data.menus);
            _this.$store.commit("updateUserInfo", response.data.data);
            _this.$router.push("/Home");
            //动态路由
            initDynamicRoutes();
          }
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
};
</script>

<style>
.el-tabs__nav-scroll {
  margin-left: 0px !important;
}
.login_container {
  height: 100%;
  width: 100%;
  background: url("../assets/img/bgimg.png");
  background-size: 100%
}
.gain {
  position: absolute;
  left: 65%;
  top: 0%;
  width: 35%;
  background: rgba(230, 230, 230, 0.5);
  min-height: 41px !important;
}
.login_box {
  width: 450px;
  height: 300px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: rgba(230, 230, 230, 0.5);
  border-radius: 15px;
}
.btus {
  display: flex;
  justify-content: flex-end;
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 415px;
  padding: 20px 18px;
}
</style>