<template>
  <!-- <div id="app"> -->
  <el-row type="flex" class="row-bg" justify="center" align="middle">
    <el-col :xs="14" :sm="12" :md="10" :lg="8" :xl="6">
      <el-form
        ref="loginForm"
        :rules="rules"
        :model="form"
        label-width="80px"
        label-position="top"
        class="form-bg"
      >
        <el-form-item label="用户名" prop="username">
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="form.password"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('loginForm')">登录</el-button>
          <el-button @click="resetForm('loginForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
  <!-- </div> -->
</template>

<script>
//导入axios,哪用哪导
import axios from "axios";

export default {
  data() {
    return {
      form: {
        username: "",
        password: ""
      },
      rules: {
        username: [
          {
            required: true,
            message: "请输入用户名",
            trigger: "blur"
          },
          { min: 3, max: 6, message: "长度在 3 到 6 个字符", trigger: "change" }
        ],
        password: [
          {
            required: true,
            message: "请输入用户名",
            trigger: "blur"
          },
          {
            min: 6,
            max: 15,
            message: "长度在 6 到 15 个字符",
            trigger: "change"
          }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          // alert("submit!");
          axios({
            url: "http://localhost:8888/api/private/v1/login",
            method: "post",
            data: this.form
            //请求的是username和password，上面data里有，所以直接用this.form代替
          }).then(({ data: { data, meta } }) => {
            // console.log(res);
            if (meta.status === 200) {
              this.$router.push("/home");
            }
          });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>

<style>
.row-bg {
  height: 100%;
  background-color: #2d434c;
}
.form-bg {
  background-color: #fff;
  border-radius: 10px;
  padding: 30px 20px;
  min-width: 400px;
}
</style>