/* eslint-disable no-debugger */
<template>
  <!-- <div class="checkout"> -->
  <div class="taskhalls">
    <!-- 弹窗 4 -->
    <div class="four">
      <el-dialog
        title="外层 Dialog 任务信息整理"
        :visible.sync="outerVisible_four"
      >
        <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
          <el-tab-pane label="任务信息" name="first">
            <div class="">
              任务名称：
              <el-input suffix-icon="el-icon-date" v-model="input_three">
              </el-input>
            </div>
          </el-tab-pane>
          <el-tab-pane label="任务编辑" name="second">
            <div class="TestFour">
              <div class="insertbutton">
                <el-button @click="addLine_four" class="addrow"
                  >添加行数</el-button
                >
              </div>
              <el-table :data="tableData_three" style="width: 100%">
                <el-table-column prop="xuhao" label="序号" width="100px">
                  <template slot-scope="scope">
                    <label v-text="scope.$index + 1"></label>
                  </template>
                </el-table-column>
                <el-table-column prop="pmid" label="Pathways">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.pmid"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="RELATED PATHWAYS">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.name"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="PUBLICATION">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.name"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="Authors">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.name"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column label="更多信息">
                  <template slot-scope="scope">
                    <el-button
                      type="primary"
                      v-model="scope.row.approvalNumber"
                      @click="bianji"
                      >编辑</el-button
                    >
                  </template>
                </el-table-column>

                <el-table-column prop="caozuo" label="操作">
                  <template slot-scope="scope">
                    <div class="insertbutton">
                      <i
                        class="el-icon-delete delete"
                        size="mini"
                        v-if="!scope.row.editing"
                        icon="el-icon-delete"
                        @click="handleDelete_three(scope.$index, scope.row)"
                      ></i>
                    </div>
                  </template>
                </el-table-column>
              </el-table>
            </div>
          </el-tab-pane>
        </el-tabs>
        <!-- 内层dialog 1-->
        <div class="three_two"></div>
        <!-- 内层dialog 1-->
        <div slot="footer" class="dialog-footer">
          <el-button
            @click="outerVisible = false"
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
            @click="outerVisible = false"
            style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
            >放弃任务</el-button
          >
        </div>
      </el-dialog>
    </div>
    <!-- 内层弹窗 开始-->
    <el-dialog
      width="30%"
      title="内层 Dialog"
      :visible.sync="innerVisible"
      :append-to-body="true"
    >
      <li
        class="center_content"
        v-for="(item, key_three) in tasklist"
        :key="key_three"
        style="margin-bottom:10px;"
      >
        <span style="float:left;width:100px;" v-text="item.name"></span>
        <el-input
          v-model="form.name"
          style="width:83%"
          v-if="item.type.indexOf('_input') >= 0"
        ></el-input>
        <el-input
          style="width:83%"
          type="textarea"
          :rows="2"
          v-model="textarea"
          v-if="item.type.indexOf('_textarea') >= 0"
        >
          <!-- v-if="item.type.indexOf('_ckeditor') >= 0" -->
        </el-input>
        <quill-editor
          v-model="content"
          ref="myQuillEditor"
          style="width:84%;margin-left:100px;"
          :options="editorOption"
          v-if="item.type.indexOf('_ckeditor') >= 0"
        >
        </quill-editor>
      </li>
      <div slot="footer" class="dialog-footer">
        <el-button
          @click="innerVisible = false"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >取 消</el-button
        >
        <el-button
          type="primary"
          @click="innerVisible = false"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >确 定</el-button
        >
      </div>
    </el-dialog>
    <!-- 内层弹窗 结束-->
    <!-- 弹窗 4 -->
  </div>
</template>
