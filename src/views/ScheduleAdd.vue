<template>
  <div id="container">
    <el-form ref="form" class="scheduleForm" :model="form" label-width="80px">
      <el-form-item label="日程名称">
        <el-input v-model="form.scheduleName"></el-input>
      </el-form-item>
      <el-form-item label="日程地点">
        <el-input v-model="form.scheduleAddr"></el-input>
      </el-form-item>
      <el-form-item label="日程时间">
        <el-input v-model="form.scheduleTime"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          type="warning"
          icon="el-icon-back"
          @click="backScheduleList"
          style="margin-right: 140px;"
        >返回列表</el-button>
        <el-button type="primary" @click="onSubmit">立即创建</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import qs from "qs";
export default {
  data() {
    return {
      form: {
        scheduleName: "",
        scheduleAddr: "",
        scheduleTime: ""
      }
    };
  },
  methods: {
    backScheduleList() {
      this.$router.replace({
        path: "/schedule-list"
      });
    },
    onSubmit() {
      let params = {
        scheduleName: this.form.scheduleName,
        scheduleAddr: this.form.scheduleAddr,
        scheduleTime: this.form.scheduleTime
      };
      params = qs.stringify(params);
      this.$axios
        .post("http://domla.xyz:8080/member/schedule", params, {
          headers: {
            "Content-Type": "application/x-www-form-urlencoded"
          }
        })
        .then(response => {
          if (response.data.code == 200) {
            this.$message({
              type: "success",
              message: "添加成功!"
            });
          } else {
            this.$message({
              type: "error",
              message: "添加失败!"
            });
          }
        });
    }
  }
};
</script>
<style>
#container {
  margin-top: 250px;
}
.scheduleForm {
  width: 500px;
  margin: 0 auto;
}
</style>


