<template>
  <div class="register">
    <section class="form_container">
      <div class="manage_tip">
        <span class="title">注册界面</span>
        <el-form
          :model="registerUser"
          :rules="rules"
          ref="registerForm"
          label-width="80px"
          class="registerForm"
        >
          <el-form-item
            label="用户名"
            prop="name"
          >
            <el-input
              type="text"
              v-model="registerUser.name"
              aria-placeholder="请输入用户名"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="邮箱"
            prop="email"
          >
            <el-input
              type="text"
              v-model="registerUser.email"
              aria-placeholder="请输入邮箱"
            ></el-input>
          </el-form-item>

          <el-form-item
            label="密码"
            prop="password"
          >
            <el-input
              type="password"
              v-model="registerUser.password"
              aria-placeholder="请输入密码"
            ></el-input>
          </el-form-item>

          <el-form-item
            label="确认密码"
            prop="password2"
          >
            <el-input
              type="password"
              v-model="registerUser.password2"
              aria-placeholder="请确认密码"
            ></el-input>
          </el-form-item>

          <el-form-item label="身份">
            <el-select
              v-model="registerUser.identity"
              aria-placeholder="请选择身份"
            >
              <el-option
                label="管理员"
                value="manager"
              ></el-option>
              <el-option
                label="员工"
                value="employee"
              ></el-option>
            </el-select>
          </el-form-item>

          <el-form-item>
            <el-button
              type="primary"
              @click="submitForm('registerForm') "
              class="submit_btn"
            >注册</el-button>
          </el-form-item>

        </el-form>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "register",
  component: {},
  data() {
    var validatePass2 = (rule, value, callback) => {
      if (value != this.registerUser.password) {
        callback(new Error("两次密码不一致"));
      } else {
        callback();
      }
    };
    return {
      registerUser: {
        name: "",
        email: "",
        password: "",
        password2: "",
        identity: ""
      },
      rules: {
        name: [
          { required: true, message: "用户名不能为空", trigger: "blur" },
          { min: 2, max: 30, message: "长度在2到30个字符之间", trigger: "blur" }
        ],
        email: [
          {
            type: "email",
            required: true,
            message: "邮箱格式不正确",
            trigger: "blur"
          }
        ],
        password: [
          { required: true, message: "密码不能为空", trigger: "blur" },
          { min: 6, max: 30, message: "长度在6到30个字符之间", trigger: "blur" }
        ],
        password2: [
          { required: true, message: "确认密码不能为空", trigger: "blur" },
          {
            min: 6,
            max: 30,
            message: "长度在6到30个字符之间",
            trigger: "blur"
          },
          { validator: validatePass2, trigger: "blur" }
        ],
        identity: []
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          const res = this.$axios.post(
            "/api/register",
            this.registerUser
          );
          if (res) {
            this.$message({
              message: "账号注册成功",
              type: "success"
            });
          }

          // .then(res => {
          //   this.$message({
          //     message: "账号注册成功",
          //     type: "success"
          //   });
          // });
        }
        this.$router.push("/login");
      });
    }
  }
};
</script>

<style scoped>
.register {
  position: relative;
  width: 100%;
  height: 100%;
  background: url(../assets/bg.jpg) no-repeat center center;
  background-size: 100% 100%;
}
.form_container {
  width: 370px;
  height: 210px;
  position: absolute;
  top: 10%;
  left: 34%;
  padding: 25px;
  border-radius: 5px;
  text-align: center;
}
.form_container .manage_tip .title {
  font-family: "Microsoft YaHei";
  font-weight: bold;
  font-size: 26px;
  color: #fff;
}
.registerForm {
  margin-top: 20px;
  background-color: #fff;
  padding: 20px 40px 20px 20px;
  border-radius: 5px;
  box-shadow: 0 5px 10px #cccccc;
}
.submit_btn {
  width: 100%;
}
</style>

