<template>
  <div class="salary">
      <el-button @click="salaryVisible = true" type="primary">添加薪资</el-button>
      <el-table
        ref="singleTable"
        :data="salary"
        highlight-current-row
        @current-change="handleCurrentChange"
        style="width: 100%">
        <el-table-column
        type="index"
        width="50">
        </el-table-column>
        <el-table-column
        property="NAME"
        label="姓名"
        >
        </el-table-column>
        <el-table-column
        property="baseWage"
        label="基本薪资"
        >
        </el-table-column>
        <el-table-column
        property="subsidy"
        label="补贴"
        >
        </el-table-column>
        <el-table-column
        label="操作"
        >
        <template slot-scope="scope">
            <el-button
            size="mini"
            @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
        </template>
        </el-table-column>
    </el-table>
    <!-- 添加信息 -->
    <el-dialog title="添加薪资" :visible.sync="salaryVisible">
        <el-form :model="salaryFrom">
            <el-form-item label="选择用户" :label-width="formLabelWidth">
                <el-select v-model="salaryFrom.id" placeholder="请选择用户">
                <template v-for="itm in userList">
                  <el-option :key="itm.id" :label="itm.name" :value="itm.id"></el-option>
                </template>
              </el-select>
            </el-form-item>
            <el-form-item label="基本薪资" :label-width="formLabelWidth">
                <el-input v-model="salaryFrom.baseWage" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="补贴" :label-width="formLabelWidth">
                <el-input v-model="salaryFrom.subsidy" auto-complete="off"></el-input>
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="salaryVisible = false">取 消</el-button>
            <el-button type="primary" @click="addSalaryFrom">确 定</el-button>
        </div>
    </el-dialog>
    <!-- 修改 -->
    <el-dialog title="修改薪资" :visible.sync="modifySalaryVisible">
        <el-form :model="modifyCurFrom">
            <el-form-item label="姓名" :label-width="formLabelWidth">
                <el-input v-model="modifyCurFrom.NAME" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="基本薪资" :label-width="formLabelWidth">
                <el-input v-model="modifyCurFrom.baseWage" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="补贴" :label-width="formLabelWidth">
                <el-input v-model="modifyCurFrom.subsidy" auto-complete="off"></el-input>
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="modifySalaryVisible = false">取 消</el-button>
            <el-button type="primary" @click="modifySalaryFrom">确 定</el-button>
        </div>
    </el-dialog>
    
  </div>
</template>
<script>
import { mapGetters, mapActions } from "vuex";
import apiSalary from "api/salary";
export default {
  name: "salarymrg",
  data() {
    return {
      salaryVisible: false,
      salaryFrom: {},
      formLabelWidth: "120px",
      modifyCurFrom: {},
      modifySalaryVisible: false,
      countFrom: {}
    };
  },
  methods: {
    ...mapActions(["salaryData", "updataSalary"]),
    handleCurrentChange() {},
    handleEdit(index, row) {
      this.modifyCurFrom = row;
      this.modifySalaryVisible = true;
    },
    addSalaryFrom() {
      this.salaryData({
        userId: this.salaryFrom.id,
        baseWage: this.salaryFrom.baseWage,
        subsidy: this.salaryFrom.subsidy
      });
      this.salaryVisible = false;
    },
    modifySalaryFrom() {
      this.updataSalary(this.modifyCurFrom);
      this.modifySalaryVisible = false;
    },
    
  },
  computed: {
    ...mapGetters(["salary", "userList"])
  },
  created() {
    this.salaryData();
  }
};
</script>

<style scoped lang="scss">
.salary {
  &-find {
    width: 50%;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
}
</style>
