/* eslint-disable no-debugger */
<template>
  <!-- <div class="checkout"> -->
  <div class="taskhalls">
    <el-dialog title="编辑任务2" :visible.sync="dialogFormVisible2" class="two">
      <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
        <el-tab-pane label="任务信息" name="first"
          ><div class="">
            任务名称：
            <el-input
              placeholder="请选择日期"
              suffix-icon="el-icon-date"
              v-model="input1"
            >
            </el-input>
            评论内容：
            <el-input
              placeholder="请输入内容"
              prefix-icon="el-icon-search"
              v-model="input2"
            >
            </el-input>
          </div>
        </el-tab-pane>
        <el-tab-pane label="任务编辑" name="second">
          <div class="TestWord">
            <div class="insertbutton">
              <el-button @click="addLine" class="addrow">添加行数</el-button>
            </div>
            <el-table :data="tableData" style="width: 100%">
              <el-table-column prop="xuhao" label="序号" width="100px">
                <template slot-scope="scope">
                  <label v-text="scope.$index + 1"></label>
                </template>
              </el-table-column>
              <el-table-column prop="drugGenericName" label="药物通用名">
                <template slot-scope="scope">
                  <el-input
                    type="textarea"
                    v-model="scope.row.drugGenericName"
                    placeholder="药物通用名"
                  ></el-input>
                </template>
              </el-table-column>
              <el-table-column prop="drugTradeName" label="药物商品名">
                <template slot-scope="scope">
                  <el-input
                    type="textarea"
                    v-model="scope.row.drugTradeName"
                    placeholder="药物商品名"
                  ></el-input>
                </template>
              </el-table-column>
              <el-table-column prop="approvalNumber" label="批准文号">
                <template slot-scope="scope">
                  <el-input
                    type="textarea"
                    v-model="scope.row.approvalNumber"
                    placeholder="批准文号"
                  ></el-input>
                </template>
              </el-table-column>
              <el-table-column prop="specifications" label="规格">
                <template slot-scope="scope">
                  <el-input
                    type="textarea"
                    v-model="scope.row.specifications"
                    placeholder="规格"
                  ></el-input>
                </template> </el-table-column
              ><el-table-column prop="originPlace" label="国家/厂家">
                <template slot-scope="scope">
                  <el-input
                    type="textarea"
                    v-model="scope.row.originPlace"
                    placeholder="国家/厂家"
                  ></el-input>
                </template>
              </el-table-column>
              <el-table-column prop="caozuo" label="操作">
                <template slot-scope="scope">
                  <div class="insertbutton">
                    <!-- <el-button
                    class="delete"
                    size="mini"
                    v-if="!scope.row.editing"
                    icon="el-icon-delete"
                    @click="handleDelete(scope.$index, scope.row)"
                    >删除
                  </el-button> -->
                    <i
                      class="el-icon-delete delete"
                      size="mini"
                      v-if="!scope.row.editing"
                      icon="el-icon-delete"
                      @click="handleDelete(scope.$index, scope.row)"
                    ></i>
                  </div>
                </template>
              </el-table-column>
            </el-table></div
        ></el-tab-pane>
      </el-tabs>

      <div slot="footer" class="dialog-footer">
        <el-button
          @click="dialogFormVisible2 = false"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >取 消</el-button
        >
        <el-button
          type="primary"
          @click="save"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >确 定</el-button
        >
        <el-button
          @click="dialogFormVisible2 = false"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >放弃任务</el-button
        >
      </div>
    </el-dialog>
  </div>
</template>
