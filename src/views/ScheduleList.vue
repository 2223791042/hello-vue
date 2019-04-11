<template>
  <div id="listContainer">
    <el-button id="addBtn" type="primary" icon="el-icon-plus" @click="addSchedule">添加日程</el-button>
    <el-table :data="tableData" style="width: 1200px;" class="mTable">
      <el-table-column label="日程ID" width="180">
        <template slot-scope="scope">{{ scope.row.scheduleId }}</template>
      </el-table-column>
      <el-table-column label="日程名称" width="180">
        <template slot-scope="scope">{{ scope.row.scheduleName }}</template>
      </el-table-column>
      <el-table-column label="日程地点" width="180">
        <template slot-scope="scope">{{ scope.row.scheduleAddr }}</template>
      </el-table-column>
      <el-table-column label="日程时间" width="180">
        <template slot-scope="scope">{{ scope.row.scheduleTime }}</template>
      </el-table-column>
      <el-table-column label="日程状态" width="180">
        <template slot-scope="scope">{{ scope.row.scheduleStatus == 0 ? '未完成' : '已完成' }}</template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="block">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="pageSizes"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      currentPage: 0,
      pageSizes: [5, 10],
      pageSize: 0,
      total: 0,
      page: 1,
      limit: 5
    };
  },
  mounted() {
    this.getScheduleList();
  },
  methods: {
    addSchedule() {
      this.$router.replace({
        path: "/schedule-add"
      });
    },
    handleEdit(index, row) {
      console.log(index, row);
    },
    handleDelete(index, row) {
      console.log(index, row);
      this.$confirm("此操作将永久删除该记录, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.$axios
            .delete("http://domla.xyz:8080/member/schedule/" + row.scheduleId)
            .then(response => {
              if (response.data.code == 200) {
                this.$message({
                  type: "success",
                  message: "删除成功!"
                });
                this.getScheduleList();
              } else {
                this.$message({
                  type: "error",
                  message: "删除失败!"
                });
              }
            });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    handleSizeChange(val) {
      this.limit = val;
      this.getScheduleList();
    },
    handleCurrentChange(val) {
      this.page = val;
      this.getScheduleList();
    },
    getScheduleList() {
      this.$axios
        .get(
          "http://domla.xyz:8080/member/schedule/list?page=" +
            this.page +
            "&limit=" +
            this.limit
        )
        .then(response => {
          this.tableData = response.data.data;
          this.total = response.data.pageInfo.total;
          this.currentPage = response.data.pageInfo.pageNum;
        });
    }
  }
};
</script>
<style>
#listContainer {
  margin-top: 100px;
}
.mTable {
  margin: 40px auto;
}
#addBtn {
  margin-right: 1100px;
}
</style>
