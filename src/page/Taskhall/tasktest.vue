<template>
  <div>
    <!-- 弹窗 1 -->
    <el-dialog title="编辑任务1" :visible.sync="dialogFormVisible1">
      <li
        class="center_content"
        v-for="(item, key1) in tasklist"
        :key="key1"
        style="margin-bottom:10px;"
      >
        <span style="float:left;width:100px;" v-text="item.name"></span>
        <el-input
          v-model="form.name"
          style="width:83%"
          v-if="item.value.indexOf('_input') > 0"
        ></el-input>
        <el-select
          v-model="form.region"
          style="width:83%"
          v-if="item.value.indexOf('_select') > 0"
        >
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
        </el-select>
        <el-input
          style="width:83%"
          type="textarea"
          :rows="2"
          v-model="textarea"
          v-if="item.value.indexOf('_textarea') > 0"
        >
        </el-input>
      </li>
      <div slot="footer" class="dialog-footer">
        <el-button
          @click="dialogFormVisible1 = false"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >取 消</el-button
        >
        <el-button
          type="primary"
          @click="dialogFormVisible1 = false"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >确 定</el-button
        >
        <el-button
          @click="dialogFormVisible1 = false"
          style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
          >放弃任务</el-button
        >
      </div>
    </el-dialog>
    <!-- 弹窗 1 -->
    <!-- 弹窗 2 -->
    <div class="two">
      <el-dialog
        title="编辑任务2"
        :visible.sync="dialogFormVisible2"
        class="two"
      >
        <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
          <el-tab-pane label="任务信息" name="first">
            <div class="">
              任务名称：
              <el-input suffix-icon="el-icon-date" v-model="input1"> </el-input>
              评论内容：
              <el-input
                type="textarea"
                prefix-icon="el-icon-search"
                v-model="input2"
              >
              </el-input>
            </div>
          </el-tab-pane>
          <el-tab-pane label="任务编辑" name="second">
            <div class="TestWord">
              <div class="insertbutton">
                <el-button @click="addLine_two" class="addrow"
                  >添加行数</el-button
                >
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
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="drugTradeName" label="药物商品名">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.drugTradeName"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="approvalNumber" label="批准文号">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.approvalNumber"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="specifications" label="规格">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.specifications"
                    ></el-input>
                  </template> </el-table-column
                ><el-table-column prop="originPlace" label="国家/厂家">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.originPlace"
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
              </el-table>
            </div>
          </el-tab-pane>
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
    <!-- 弹窗 2 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 弹窗 1
      content: null,
      editorOption: {
        action: '/img/uploadImg ', // 必填参数 图片上传地址
        methods: 'post', // 必填参数 图片上传方式
        name: 'upload_file' // 必填参数 文件的参数名
      },
      activeName: 'first',
      input1: '',
      input2: '',
      input_three: '',
      tableData: [
        {
          drugGenericName: '',
          caozuo: '',
          approvalNumber: '',
          specifications: '',
          drugTradeName: '',
          originPlace: ''
        }
      ],
      tableData_three: [
        {
          pmid: '',
          name: '',
          accessoryId: '',
          caozuo: ''
        }
      ],
      tableData_four: [
        {
          Pathways: '',
          relatedPathways: '',
          publication: '',
          authors: '',
          caozuo: ''
        }
      ],
      dialogTableVisible: false,
      dialogFormVisible1: false,
      dialogFormVisible2: false,
      outerVisible: false,
      innerVisible: false,
      outerVisible_four: false,
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      textarea: '',
      formLabelWidth: '100px',
      tasklist: [],
      // 弹窗 2
      value1: [],
      options: [
        {
          value: '选项1',
          label: '位点基本信息位点基本信息位点基本信息位点基本信息位点基本信息'
        },
        {
          value: '选项2',
          label:
            '药物基本信息位点基本信息位点基本信息位点基本信息位点基本信息位点基本信息'
        },
        {
          value: '选项3',
          label:
            '基因基本信息基因基本信息基因基本信息基因基本信息基因基本信息基因基本信息基因基本信息'
        },
        {
          value: '选项4',
          label: '疾病基本信息'
        },
        {
          value: '选项5',
          label: '项目基本信息'
        }
      ],
      id: [],
      name: '',
      taskTitleUrl: '',
      btntxt: '编辑',
      disabled: false,
      currentPage4: 1,
      input: '',
      input3: '',
      select: '',
      // 中间八个分类 1
      genenum: '',
      drugnum: '',
      drugGenePairnum: '',
      authoritynum: '',
      drugLabelsnum: '',

      // 中间八个分类 2
      total: '',
      articleTitle: '', // 小标题
      articleLinkUrl: '', // 小标题链接=具体内容页面
      columnTitle: '', // 分组大标题
      columnLinkUrl: '', // 分组大标题链接【最新事件，最新研究内容，公告】
      date: new Date(),
      error: false,
      home: [],
      loading: true,
      notify: '1',
      dialogVisible: false,
      timer: '',
      // 接收最新事件列表信息
      topNews: [],
      // 接收最新研究内容列表信息
      newContent: [],
      // taskHall: [], // 暂存请求到的任务大厅数据
      taskhall: [], // 暂存请求到的任务大厅数据
      currentPage: 1, // 当前页面
      alltotal: '', // 最多条数
      pageSize: 3 // 每页15条
    }
  }
}
</script>

<style scoped>
</style>

