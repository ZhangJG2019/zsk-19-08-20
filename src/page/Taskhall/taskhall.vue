/* eslint-disable no-debugger */
<template>
  <!-- <div class="checkout"> -->
  <div class="taskhalls">
    <y-header> <div slot="nav"></div></y-header>
    <div class="activity-panel">
      <ul class="box">
        <!-- 左侧内容区域  start-->
        <li style="margin-right:10px;background-color:#fff;">
          <div class="main_content">
            <!-- 顶部筛选条件 1-->
            <div class="task_classify clearfix">
              <ul class="task_class " style="border-bottom:1px solid #ccc">
                <strong class="fl">任务类型 :</strong>
                <li class="fl anow"><a>全部</a></li>
                <li class="fl"><a>基本信息类</a></li>
                <li class="fl"><a>注释类信息</a></li>
                <li class="fl"><a>文献原文指标分解整理</a></li>
                <li class="fl"><a>上传</a></li>
                <li class="fl"><a>数据统计</a></li>
              </ul>
              <!-- <ul class="task_power">
                <span class="fl">任务权限 :</span>
                <li class="fl actived">全部</li>
                <li class="fl">完全开放</li>
                <li class="fl">勋章开放</li>
              </ul> -->
              <el-select
                v-model="value1"
                multiple
                style="width:800px;"
                class="taskchoose"
              >
                <el-option
                  id="task"
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                  style="overflow: auto;height:36px;"
                >
                </el-option>
              </el-select>
            </div>
            <!-- 顶部筛选条件 2-->
            <!-- 左侧具体内容区 1-->
            <div class="slide_center news">
              <ul>
                <!-- 任务搜索框 1-->
                <li style="float:right;margin-bottom:13px">
                  <el-autocomplete
                    icon="search"
                    v-model="input"
                    minlength="1"
                    maxlength="100"
                    :fetch-suggestions="querySearchAsync"
                    @select="handleSelect"
                    :on-icon-click="handleIconClick"
                    @keydown.enter.native="handleIconClick"
                  >
                  </el-autocomplete>
                </li>
                <!-- 任务搜索框 2-->
                <!-- 任务大厅具体列表数据展示 1-->
                <li
                  class="center_content"
                  v-for="(item, key) in taskhall.slice(
                    (currentPage - 1) * pageSize,
                    currentPage * pageSize
                  )"
                  :key="key"
                  :id="forId(key)"
                  @current-change="handleCurrentChange"
                >
                  <img
                    src="../../../static/images/liwu.png"
                    alt=""
                    style="color:blue;float:left!important; display:inline-block;"
                  />
                  <a
                    @click="See(item.articleLinkUrl)"
                    class="title"
                    style="float:left!important; display:inline-block;"
                    v-text="item.name"
                  ></a>
                  <!-- <el-button
                    :id="forId2(key)"
                    type="primary"
                    @click="lingqu(item.id)"
                    :disabled="disabled"
                    v-if="disabled == false"
                    style="margin-left:15px"
                    >领取
                  </el-button> -->
                  <el-button
                    :id="forId2(key)"
                    type="button"
                    @click="bianji1(item.id)"
                    v-if="disabled == false"
                    v-text="btntxt"
                  >
                  </el-button>
                </li>
                <!-- 任务大厅具体列表数据展示 2-->
                <!-- 分页 1-->
                <li
                  style="height:60px;line-height:60px;  "
                  class="center_content"
                >
                  <el-pagination
                    background
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="currentPage"
                    :page-sizes="[3, 6, 9]"
                    :page-size="pageSize"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="taskhall.length"
                    style="float:right;margin:25px 10px 0 0;"
                  >
                  </el-pagination>
                </li>
                <!-- 分页 2-->
              </ul>
            </div>
            <!-- 左侧具体内容区 1-->
          </div>
        </li>
        <!-- 左侧内容区域  end-->
        <!-- 右侧内容区域 start-->
        <li class="side-left left" style="margin-right:10px;">
          <div class="side_left ">
            <!-- 帮助指南 -->
            <div class=" news " style="width:326px;">
              <h2>
                <a @click="See(columnLinkUrl)"><strong>帮助指南</strong></a>
              </h2>
              <ul class="left_ul left_content">
                <li v-for="(item, key) in topNews" :key="key">
                  <span v-text="item.id"></span>
                  <a
                    @click="See(item.articleLinkUrl)"
                    v-text="item.articleTitle"
                  >
                  </a>
                </li>
              </ul>
            </div>
            <!-- 常见问题 -->
            <div class=" news" style="width:326px;margin-top:18px">
              <h2>
                <a @click="See(columnLinkUrl)"><strong>常见问题</strong></a>
              </h2>
              <ul id="newlog" class="left_content">
                <li v-for="(item, key) in newContent" :key="key">
                  <span v-text="item.id" class="new_content "></span>
                  <a
                    @click="See(item.articleLinkUrl)"
                    v-text="item.articleTitle"
                  ></a>
                </li>
              </ul>
            </div>
          </div>
        </li>
        <!-- 右侧内容区域 end-->
      </ul>
    </div>
    <y-footer></y-footer>
    <!-- 弹窗 3 -->
    <div class="three">
      <el-dialog title="外层 Dialog 编辑任务3" :visible.sync="outerVisible">
        <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
          <el-tab-pane label="任务信息" name="first">
            <div class="">
              任务名称：
              <el-input suffix-icon="el-icon-date" v-model="input_three">
              </el-input>
            </div>
          </el-tab-pane>
          <el-tab-pane label="任务编辑" name="second">
            <div class="TestThree">
              <div class="insertbutton">
                <el-button @click="addLine_three" class="addrow"
                  >添加行数</el-button
                >
              </div>
              <el-table :data="tableData_three" style="width: 100%">
                <el-table-column prop="xuhao" label="序号" width="100px">
                  <template slot-scope="scope">
                    <label v-text="scope.$index + 1"></label>
                  </template>
                </el-table-column>
                <el-table-column prop="pmid" label="PMID">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.pmid"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="文献名称">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.name"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column label="文献内容提取">
                  <template slot-scope="scope">
                    <el-button
                      type="primary"
                      v-model="scope.row.approvalNumber"
                      @click="bianji"
                      >编辑</el-button
                    >
                  </template>
                </el-table-column>
                <el-table-column prop="accessoryId" label="文献上传">
                  <template slot-scope="scope">
                    <el-select v-model="scope.row.accessoryId"></el-select>
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
      title="内层 Dialog3（不带three）"
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
    <!-- 弹窗 3 -->
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
              <el-table :data="tableData_four" style="width: 100%">
                <el-table-column prop="xuhao" label="序号" width="100px">
                  <template slot-scope="scope">
                    <label v-text="scope.$index + 1"></label>
                  </template>
                </el-table-column>
                <el-table-column prop="pmid" label="Pathways">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.pathways"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="RELATED PATHWAYS">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.relatedPathways"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="PUBLICATION">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.publication"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column prop="name" label="Authors">
                  <template slot-scope="scope">
                    <el-input
                      type="textarea"
                      v-model="scope.row.authors"
                    ></el-input>
                  </template>
                </el-table-column>
                <el-table-column label="更多信息">
                  <template slot-scope="scope">
                    <el-button
                      type="primary"
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
                        @click="handleDelete_four(scope.$index, scope.row)"
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
            @click="outerVisible_four = false"
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
            @click="outerVisible_four = false"
            style="margin:3px 0 0 10px; width:90px;height:40px;font-size:16px;vertical-align:middle;"
            >放弃任务</el-button
          >
        </div>
      </el-dialog>
    </div>
    <!-- 内层弹窗 开始-->
    <el-dialog
      width="30%"
      title="内层 Dialog 任务信息整理"
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
<script>
import YShelf from '/components/shelf'
import YButton from '/components/YButton'
import YHeader from '/common/header'
import YFooter from '/common/footer'
// import { taskHall } from '/api/index.js' // zsk模拟接口
import 'jquery'
import 'element-ui'
import axios from 'axios'
import { quillEditor } from 'vue-quill-editor' // 调用编辑器
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'
export default {
  name: 'TestWorld',

  // 生命周期函数
  created() {
    this.getHelp() // 帮助指南
    this.getProblems() // 常见问题
    this.getNum() // 左侧内容数字
    this.getTaskList() // 任务大厅列表
    // this.test() // zsk连接测试
  },
  // name: 'hello',
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
          pathways: '',
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
  },
  mounted() {
    // 菜单切换高亮显示 1
    $(document).ready(function() {
      $('.task_class li').click(function() {
        $(this)
          .addClass('anow')
          .siblings()
          .removeClass('anow')
      })
    })
    // 菜单切换高亮显示 2
  },
  methods: {
    // 富文本编辑器
    onEditorReady(editor) {},
    handleClick(tab, event) {
      console.log(tab, event)
    },
    addLine_two() {
      // 添加行数
      var newValue = {
        drugGenericName: '',
        drugTradeName: '',
        approvalNumber: '',
        specifications: '',
        originPlace: '',
        caozuo: ''
      }
      // 添加新的行数
      this.tableData.push(newValue)
    },
    addLine_three() {
      // 添加行数
      var newValue = {
        pmid: '',
        name: '',
        accessoryId: '',
        caozuo: ''
      }
      // 添加新的行数
      this.tableData_three.push(newValue)
    },
    addLine_four() {
      // 添加行数
      var newValue = {
        pathways: '',
        relatedPathways: '',
        publication: '',
        authors: '',
        caozuo: ''
      }
      // 添加新的行数
      this.tableData_four.push(newValue)
    },
    // 删除行4
    handleDelete_four(index) {
      console.log(index)
      // 删除行数
      this.tableData_four.splice(index, 1)
    },
    // 删除行3
    handleDelete_three(index) {
      console.log(index)
      // 删除行数
      this.tableData_three.splice(index, 1)
    },
    // 删除行2
    handleDelete(index) {
      console.log(index)
      // 删除行数
      this.tableData.splice(index, 1)
    },
    // 弹窗中确定提交按钮
    save() {
      this.outerVisible_four = false
      // 这部分应该是保存提交你添加的内容
      console.log(JSON.stringify(this.tableData_four))
    },
    // 给任务大厅数据列表动态添加id
    forId: function(index) {
      return 'forid_' + index
    },
    //  给任务大厅数据列表中按钮动态添加id
    forId2: function(index) {
      return 'forid2_' + index
    },
    // 获取任务大厅数据列表
    getTaskList() {
      var url = 'static/data/taskhall.json'
      axios({
        method: 'get',
        url: url,
        rows: '3',
        page: '1'
      }).then(res => {
        // console.log(res)
        // console.log(res.data[0])
        this.taskhall = res.data
        this.name = res.data[0].name
        this.taskTitleUrl = res.data[0].taskTitleUrl
        this.id = res.data.id
      })
    },
    bianji1() {
      this.outerVisible_four = true
    },
    bianji() {
      this.innerVisible = true
      this.tasklist = []
      // var url = 'static/data/taskhall.json'
      // axios({
      //   method: 'get',
      //   url: url
      // }).then(res => {
      //   // debugger
      //   console.log(res)
      //   let data1 = res.data
      //   for (var key in data1) {
      //     let s = {
      //       name: data1[key],
      //       value: key
      //     }
      //     this.tasklist.push(s)
      //   }
      // })

      let data2 = {
        citation_textarea: 'Citation',
        history_ckeditor3: 'History',
        summary_ckeditor1: 'Summary',
        description_ckeditor2: 'Description',
        therapaeutic_textarea: 'Therapeutic Categories'
      }
      for (var key3 in data2) {
        // console.log(key3 + ' : ' + data2[key3])
        let id = key3.substring(0, key3.lastIndexOf('_'))
        let type = key3.substring(key3.lastIndexOf('_'), key3.length)
        let s = {
          name: data2[key3],
          value: id,
          type: type
        }
        this.tasklist.push(s)
      }
    },
    lingqu(id) {
      this.disabled = true
    },
    test() {
      // 链接知识库跨域测试
      // let data = new FormData()
      // data.append('test', 'hello world')
      // taskHall(data).then(res => {
      //   console.log(res)
      // })
    },
    // 任务列表数据分页
    handleSizeChange(val) {
      // console.log(`每页 ${val} 条`)
      this.pageSize = val // 动态改变
      this.getTaskList() // 重新获取数据列表
    },
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`)
      this.currentPage = val // 动态改变
      this.getTaskList() // 重新获取数据列表
    },

    // 查询信息
    handleIconClick(ev) {
      if (this.$route.path === '/search') {
        this.$router.push({
          path: '/refreshsearch',
          query: {
            key: this.input
          }
        })
      } else {
        this.$router.push({
          path: '/search',
          query: {
            key: this.input
          }
        })
      }
    },
    querySearchAsync(queryString, cb) {
      if (this.input === undefined) {
        cb([])
        return
      }
      this.input = this.input.trim()
      if (this.input === '') {
        cb([])
        return
      } else {
        this.loadAll()
        setTimeout(() => {
          cb(this.searchResults)
        }, 300)
      }
    },
    handleSelect(item) {
      this.input = item.value
    },
    // 基因
    gene() {
      this.$router.push({
        path: '/gene'
      })
    },
    // 获取各分类标签数量
    getNum() {
      var url = 'static/data/home_center.json'
      axios({
        method: 'get',
        url: url
      }).then(res => {
        // console.log(res)
        // console.log(res.data[0].genenum)
        this.genenum = res.data[0].genenum
        this.drugnum = res.data[0].drugnum
        this.drugGenePairnum = res.data[0].drugGenePairnum
        this.authoritynum = res.data[0].authoritynum
        this.drugLabelsnum = res.data[0].drugLabelsnum
      })
    },
    // 帮助指南
    getHelp() {
      // var topNew = '最新事件'
      // var url = '/apis/cms/api/getColumnNewList?title=' + topNew
      var url = 'static/data/home_gethelp.json'
      axios({
        method: 'get',
        url: url
      }).then(res => {
        // console.log(res)
        // console.log(res.data[0].columnTitle)
        // 把获得好的最新事件 赋予topNews 给成员
        this.topNews = res.data
        this.columnLinkUrl = res.data[0].columnLinkUrl
      })
    },
    // 常见问题
    getProblems() {
      // var newContent = '常见问题'
      // var url = '/apis/cms/api/getColumnNewList?title=' + newContent
      var url = 'static/data/home_newContent.json'
      axios({
        method: 'get',
        url: url
      }).then(res => {
        // 把获得好的最新事件 赋予 给NewContent成员
        this.newContent = res.data
        this.columnLinkUrl = res.data[0].columnLinkUrl
      })
    },
    // cms页面跳转
    See(e) {
      window.location = e
    }
  },
  components: {
    YShelf,
    YButton,
    YHeader,
    YFooter,
    quillEditor
  }
}
</script>
<style lang="scss" rel="stylesheet/scss" scoped>
//中间顶部筛选条件 1
// .el-select .el-input {
//   width: 130px;
// }
.input-with-select .el-input-group__prepend {
  background-color: #fff;
}
.task_classify {
  width: 100%;
  background-color: #fafafa;
  height: 100px;
  padding: 15px;
  box-sizing: border-box;
}
.task_classify .task_power {
  margin-top: 10px;
}
.task_classify ul {
  height: 36px;
  line-height: 20px;
}
.task_classify ul li {
  margin: 0 10px;
  padding: 0 20px;
  cursor: pointer;
}
.task_class li.anow {
  border: 2px solid #13cb6a;
}
.task_classify ul li.fl {
  float: left;
}
//中间顶部筛选条件 2
// .el-textarea .el-textarea__inner {
//   // 然后找到对应的类名，在这里将拉伸去掉即可
//   resize: none;
// }
#box {
  display: flex;
}
.left {
  width: 326px;
}
.news h2 {
  height: 56px;
  line-height: 56px;
  background: #ffffff;
  color: rgb(66, 126, 236);
  border: 1px solid #ddd;
  border-bottom: 3px solid #cdcdcd;
  border-radius: 3px 3px 0 0;
  -webkit-border-radius: 3px 3px 0 0;
}
.news h2 a {
  color: rgb(66, 126, 236);
  box-sizing: border-box;
  display: block;
  text-decoration: none;
}
.news h2 strong {
  font: bold 20px 'microsoft yahei';
  height: 56px;
  padding: 0px 24px;
  box-sizing: border-box;
}
.news h2 strong:hover {
  border-bottom: 3px solid #417eec;
  padding-bottom: 14px;
}
.left_content {
  border: 1px solid #ddd;
  border-top: 0;
  padding-top: 10px;
  box-sizing: border-box;
  background-color: #fff;
}
.news .left_ul {
  height: 334px;
  overflow: hidden;
}
.news #newlog {
  height: 386px;
  overflow: hidden;
  padding-top: 15px;
  box-sizing: border-box;
}
.news ul li {
  width: 263px;
  height: 40px;
  line-height: 40px;
  padding: 0 0 0 20px;
  float: left;
  overflow: hidden;
}
.news ul li a {
  color: #666;
}
.side_left ul li {
  width: 300px;
  box-sizing: border-box;
  font-size: 14px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.side_left ul span {
  margin-right: 15px;
  width: 10px;
  height: 10px;
}
.slide_center ul {
  height: 100%;
  padding: 15px;
  box-sizing: border-box;
}
.slide_center ul .num {
  position: absolute;
  bottom: 5px;
  right: 20px;
  font-size: 30px;
  color: white;
}
.slide_center ul li.center_content {
  width: 870px;
  height: 120px;
  box-sizing: border-box;
  position: relative;
  cursor: pointer;
  border-top: 1px solid #ccc;
}
.slide_center ul li.center_content img {
  width: 50px;
  height: 50px;
  float: left;
  margin-top: 35px;
}
.slide_center ul li a.title {
  font-size: 20px;
  float: left;
  margin-left: 1.25rem;
  margin-top: 2.5rem;
}
.slide_center ul li button {
  height: 2.5rem;
  width: 7rem;
  float: right;
  margin: 2.5rem;
  border-radius: 1.5625rem;
}
.side_left .news .right_content {
  height: 217px;
  background-color: #fafafa;
}
.side_left .right_content li {
  width: 260px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.tel {
  margin-top: 20px;
  background-color: #fff;
  border-left: 1px solid #ccc;
  // padding-bottom: 20px;
}
.Message {
  background-color: #fff;
  margin-top: 20px;
}
.Message h2 {
  line-height: 56px;
  font-size: 15px;
  color: black;
  background-color: #f1f1f1;
}
.el-form--label-top .el-form-item__label {
  font-size: 16px !important;
  padding-left: 10px;
}
.el-form-item {
  margin: 10px 0 10px 0;
}
.left_form {
  margin-left: 10px;
}
.Message .left_form label {
  font-size: 14px;
  margin: 15px 0 5px 0 !important;
  display: inline-block;
}
.left_form input {
  height: 26px;
  width: 238px;
  // background-color: #f8f8f8;
}
.left_form .auth_code {
  vertical-align: middle;
  width: 50px;
  float: left;
}
.left_form a {
  vertical-align: middle;
  width: 95px;
  height: 30px;
  background-color: #17c2ce;
  display: inline-block;
  line-height: 30px;
  padding-left: 15px;
  box-sizing: border-box;
}
/* 底部业务划分区域 */
.guide {
  height: 120px;
}
.guide_ul {
  float: left;
  margin-left: 10px;
}
.guide_ul li {
  float: left;
  width: 285px;
  height: 76px;
  margin: 20px 0 0 20px;
  cursor: pointer;
}
.guide_ul li:first-child {
  margin-left: 0;
}
.guide_ul li i {
  font-size: 45px;
  color: white;
  margin: 15px 0 0 15px;
  float: left;
}
.guide_ul li p {
  margin-left: 16px;
  height: 76px;
  line-height: 76px;
  /* display: inline-block; */
  font-size: 20px;
  color: #fff;
  float: left;
}
/* .partners{height: 94px;} */
/* 底部轮播图 */
* {
  margin: 0px;
  padding: 0px;
}
#box {
  width: 1200px;
  height: 65px;
  overflow: hidden;
  position: relative;
  background: #f2f2f2;
}
#box ul {
  position: absolute;
  left: 0px;
  right: 0px;
}
#box ul li {
  float: left;
  list-style: none;
  width: 118px;
  height: 58px;
  padding: 10px 0;
  box-sizing: border-box;
}
// 自己加的
.center_content {
  position: relative;
}
.guide {
  width: 1200px;
}
.home {
  display: flex;
  flex-direction: column;
}
.no-info {
  padding: 100px 0;
  text-align: center;
  font-size: 30px;
  display: flex;
  flex-direction: column;
  .no-data {
    align-self: center;
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.page {
  position: absolute;
  width: 100%;
  top: 470px;
  z-index: 30;
  .dots {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    .dot-active {
      display: inline-block;
      width: 15px;
      height: 15px;
      background-color: whitesmoke;
      border-radius: 8px;
      margin-right: 10px;
      cursor: pointer;
    }
    .dot {
      opacity: 0.2;
    }
  }
}
.activity-panel {
  width: 1220px;
  margin: 0 auto;
  .box {
    overflow: hidden;
    position: relative;
    z-index: 0;
    margin-top: 15px;
    box-sizing: border-box;
    // border: 1px solid rgba(0, 0, 0, 0.14);
    border-radius: 8px;
    background: #ededed;
    box-shadow: 0 3px 8px -6px rgba(0, 0, 0, 0.1);
  }
  .content {
    float: left;
    position: relative;
    box-sizing: border-box;
    width: 25%;
    height: 200px;
    text-align: center;
  }
  .content ::before {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    box-sizing: border-box;
    border-left: 1px solid #f2f2f2;
    border-left: 1px solid rgba(0, 0, 0, 0.1);
    width: 100%;
    height: 100%;
    content: '';
    pointer-events: none;
  }
  .i {
    width: 305px;
    height: 200px;
  }
  .cover-link {
    cursor: pointer;
    display: block;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 4;
    background: url(data:image/gif;base64,R0lGODlhAQABAIAAAP///////yH5BAEHAAEALAAAAAABAAEAAAICTAEAOw==)
      repeat;
  }
  a {
    color: #5079d9;
    cursor: pointer;
    transition: all 0.15s ease-out;
    text-decoration: none;
  }
  a:hover {
    // box-shadow: inset 0 0 38px rgba(0, 0, 0, 0.08);
    transition: all 0.15s ease;
  }
}
.banner,
.banner span,
.banner div {
  font-family: 'Microsoft YaHei';
  transition: all 0.3s;
  transition-timing-function: linear;
}
.banner {
  cursor: pointer;
  perspective: 3000px;
  position: relative;
  z-index: 19;
  margin: 0 auto;
  width: 1220px;
}
.bg {
  position: relative;
  width: 1220px;
  height: 500px;
  margin: 20px auto;
  background-size: 100% 100%;
  border-radius: 10px;
  transform-style: preserve-3d;
  transform-origin: 50% 50%;
  transform: rotateY(0deg) rotateX(0deg);
  & div {
    position: relative;
    height: 100%;
    width: 100%;
  }
}
.img1 {
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  border-radius: 10px;
}
.img2 {
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  bottom: 5px;
  left: 0;
  background-size: 95% 100%;
  border-radius: 10px;
}
.img3 {
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  border-radius: 10px;
}
.a {
  z-index: 20;
  transform: translateZ(40px);
}
.b {
  z-index: 20;
  transform: translateZ(30px);
}
.c {
  transform: translateZ(0px);
}
.sk_item {
  width: 170px;
  height: 225px;
  padding: 0 14px 0 15px;
  > div {
    width: 100%;
  }
  a {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    transition: all 0.3s;
    &:hover {
      transform: translateY(-5px);
    }
  }
  img {
    width: 130px;
    height: 130px;
    margin: 17px 0;
  }
  .sk_item_name {
    color: #999;
    display: block;
    max-width: 100%;
    _width: 100%;
    overflow: hidden;
    font-size: 12px;
    text-align: left;
    height: 32px;
    line-height: 16px;
    word-wrap: break-word;
    word-break: break-all;
  }
  .sk_item_price {
    padding: 3px 0;
    height: 25px;
  }
  .price_new {
    font-size: 18px;
    font-weight: 700;
    margin-right: 8px;
    color: #f10214;
  }
  .price_origin {
    color: #999;
    font-size: 12px;
  }
}
.box {
  overflow: hidden;
  position: relative;
  z-index: 0;
  margin-top: 29px;
  box-sizing: border-box;
  // border: 1px solid rgba(0, 0, 0, 0.14);
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 3px 8px -6px rgba(0, 0, 0, 0.1);
}
ul.box {
  display: flex;
  li {
    // flex: 1;
    img {
      display: block;
      width: 305px;
      height: 200px;
    }
  }
}
.mt30 {
  margin-top: 30px;
}
.hot {
  display: flex;
  > div {
    flex: 1;
    width: 25%;
  }
}
.floors {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  .imgbanner {
    width: 50%;
    height: 430px;
    .cover-link {
      cursor: pointer;
      display: block;
      position: absolute;
      top: 60px;
      left: 0;
      width: 50%;
      height: 430px;
      z-index: 4;
      background: url(data:image/gif;base64,R0lGODlhAQABAIAAAP///////yH5BAEHAAEALAAAAAABAAEAAAICTAEAOw==)
        repeat;
    }
    .cover-link:hover {
      box-shadow: inset 0 0 38px rgba(0, 0, 0, 0.08);
      transition: all 0.15s ease;
    }
  }
  img {
    display: block;
    width: 100%;
    height: 100%;
  }
}
// 清除浮动
.clearfix {
  zoom: 1;
} /*==for IE6/7 Maxthon2==*/
.clearfix :after {
  clear: both;
  content: '.';
  display: block;
  width: 0;
  height: 0;
  visibility: hidden;
} /*==for FF/chrome/opera/IE8==*/
</style>

<style>
.insertbutton .addrow {
  width: 90px;
  height: 30px;
  border-radius: 15px;
  color: #20a0ff;
  border-color: #20a0ff;
  float: right;
}
.insertbutton .delete {
  /* width: 90px;
  height: 30px;
  border-radius: 15px; */
  font-size: 20px;
  color: hsla(3, 82%, 69%, 0.77);
}
.two .el-dialog--small,
.three .el-dialog--small,
.four .el-dialog--small,
.five .el-dialog--small,
.six .el-dialog--small,
.seven .el-dialog--small {
  width: 86% !important;
}

.el-dialog__header {
  background-color: #eee;
  padding: 10px;
}

.el-tag .el-select__tags-text {
  float: left !important;
}
.el-tag .el-icon-close {
  float: left !important;
  margin-top: 4px;
}
.el-icon-caret-top {
  margin-top: 2px !important;
}
.el-select__tags {
  float: left;
  height: 60px;
  overflow: auto;
}
.el-select__tags::-webkit-scrollbar {
  /*滚动条整体样式*/
  width: 4px; /*高宽分别对应横竖滚动条的尺寸*/
  height: 4px;
}
.el-select__tags::-webkit-scrollbar-thumb {
  /*滚动条里面小方块*/
  border-radius: 5px;
  -webkit-box-shadow: inset 0 0 5px rgba(255, 255, 255, 0.2);
  background: rgba(255, 255, 255, 0.2);
}

.el-select__tags::-webkit-scrollbar-track {
  /*滚动条里面轨道*/
  -webkit-box-shadow: inset 0 0 5px rgba(255, 255, 255, 0.2);
  border-radius: 0;
  background: rgba(255, 255, 255, 0.1);
}
.el-select__tags span.el-tag {
  float: left;
}
</style>
