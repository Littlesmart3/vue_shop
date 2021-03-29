<template>
  <!-- <a-spin :spinning="is_loading" class="container"> -->
  <div class="background">
    <!-- 页面标题:学员统计报表 -->
    <div>
      <span class="title">学员统计报表</span>
    </div>
    <!-- 概况和明细分页 -->
    <div class="card-container marginTop-20">
      <a-tabs type="card" animated>
        <a-tab-pane key="1">
          <template slot="tab">
            <span class="tab-width-70">概况</span>
          </template>
          <!-- 搜索框、导出框 -->
          <a-form
            layout="inline"
            ref="GeneralSituationFormRef"
            :model="GeneralSituationForm"
          >
            <!-- 负责人搜索框 -->
            <a-form-item
              mode="tags"
              class="width-22"
              placeholder="Tags Mode"
              @change="handleChange"
              label="负责人"
              prop="teacher"
            >
              <a-select
                placeholder="请选择"
                class="width-160"
                v-decorator="['principal']"
                v-model="GeneralSituationForm.teacher"
              >
                <a-select-option v-for="item in branch_office" :key="item.id">{{
                  item.value
                }}</a-select-option>
              </a-select>
            </a-form-item>
            <!-- 统计时间搜索框 -->
            <a-form-item class="width-22" label="统计时间" prop="date">
              <a-date-picker
                class="width-160"
                placeholder="请选择"
                v-decorator="['date-picker']"
                v-model="GeneralSituationForm.date"
              />
            </a-form-item>
            <!-- 搜索按钮 -->
            <div class="button-group">
              <a-button type="primary" class="search_group" html-type="submit"
                >搜索</a-button
              >
              <a-button
                type="primary"
                class="search_group"
                @click="generalSituationExport"
                >导出</a-button
              >
              <span
                class="check-span-clear"
                @click="resetForm(GeneralSituationFormRef)"
                >清空搜索条件</span
              >
            </div>
          </a-form>

          <!-- 负责人-统计所有分社的学员总数据列表 -->

          <a-table class="marginTop-20" bordered>
            <a-table-column align="center" title="负责人"></a-table-column>
            <a-table-column align="center" title="学院数"></a-table-column>
            <a-table-column align="center" title="在籍数"></a-table-column>
            <a-table-column align="center" title="过期数"></a-table-column>
            <a-table-column align="center" title="团报数"></a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>已加数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">跟进状态为“已加”的学员数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>其他城市已加数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class=""
                      >分社为“其他/深圳/无”、且跟进状态为“已加”的学员数</span
                    >
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center" title="在籍已加数"></a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>在籍加进率</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">在籍加进率=在籍已加数/在籍数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>在籍续费数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class=""
                      >统计时间内，因“在籍续费”回访类型出现在回访列表的学员数，一个人在统计时间内多次出现记为多人</span
                    >
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>过期续费数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class=""
                      >统计时间内，因“过期续费”回访类型出现在回访列表的学员数，一个人在统计时间内多次出现记为多人</span
                    >
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>资料完善数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">最近学习时间”在统计时间内的学员数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>看课数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">最近学习时间”在统计时间内的学员数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
          </a-table>
          <a-pagination
            class="pagination"
            :total="total"
            :show-total="total => `共${total}条，每页20条`"
            :page-size="20"
            :default-current="1"
          />
        </a-tab-pane>
        <a-tab-pane key="2">
          <template slot="tab">
            <span class="tab-width-70">明细</span>
          </template>
          <!-- 搜索框、导出框 -->
          <a-form layout="inline">
            <!-- 分社搜索框 -->
            <a-form-item
              mode="tags"
              class="width-22"
              placeholder="Tags Mode"
              @change="handleChange"
              label="分社"
            >
              <a-select placeholder="请选择" class="width-160">
                <a-select-option v-for="item in branch_office" :key="item.id">{{
                  item.value
                }}</a-select-option>
              </a-select>
            </a-form-item>
            <!-- 负责人搜索框 -->
            <a-form-item
              mode="tags"
              class="width-22"
              placeholder="Tags Mode"
              @change="handleChange"
              label="负责人"
            >
              <a-select placeholder="请选择" class="width-160">
                <a-select-option v-for="item in branch_office" :key="item.id">{{
                  item.value
                }}</a-select-option>
              </a-select>
            </a-form-item>
            <!-- 负责班主任搜索框 -->
            <a-form-item
              mode="tags"
              class="width-22"
              placeholder="Tags Mode"
              @change="handleChange"
              label="负责班主任"
            >
              <a-select placeholder="请选择" class="width-160">
                <a-select-option v-for="item in branch_office" :key="item.id">{{
                  item.value
                }}</a-select-option>
              </a-select>
            </a-form-item>
            <!-- 统计时间搜索框 -->
            <a-form-item class="width-22" label="统计时间">
              <a-date-picker class="width-160" placeholder="请选择" />
            </a-form-item>
            <!-- 搜索按钮 -->
            <div class="button-group">
              <a-button
                type="primary"
                class="search_group"
                @click="detailSeacrh"
                >搜索</a-button
              >
              <a-button
                type="primary"
                class="search_group"
                @click="detailExport"
                >导出</a-button
              >
              <span class="check-span-clear" @click="resetForm"
                >清空搜索条件</span
              >
            </div>
          </a-form>

          <!-- 负责人-统计所有分社的学员总数据列表 -->

          <a-table class="marginTop-20" bordered>
            <a-table-column align="center" title="负责人"></a-table-column>
            <a-table-column align="center" title="负责班主任"></a-table-column>
            <a-table-column align="center" title="学员数"></a-table-column>
            <a-table-column align="center" title="在籍数"></a-table-column>
            <a-table-column align="center" title="过期数"></a-table-column>
            <a-table-column align="center" title="团报数"></a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>已加数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">跟进状态为“已加”的学员数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>其他城市已加数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class=""
                      >分社为“其他/深圳/无”、且跟进状态为“已加”的学员数</span
                    >
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center" title="在籍已加数"></a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>在籍加进率</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">在籍加进率=在籍已加数/在籍数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>在籍续费数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class=""
                      >统计时间内，因“在籍续费”回访类型出现在回访列表的学员数，一个人在统计时间内多次出现记为多人</span
                    >
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>过期续费数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class=""
                      >统计时间内，因“过期续费”回访类型出现在回访列表的学员数，一个人在统计时间内多次出现记为多人</span
                    >
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>资料完善数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">最近学习时间”在统计时间内的学员数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
            <a-table-column align="center">
              <span slot="title">
                <span>看课数</span>
                <a-popover class="inline" placement="top">
                  <p slot="content">
                    <span class="">最近学习时间”在统计时间内的学员数</span>
                  </p>
                  <div>
                    <a-icon
                      type="question-circle"
                      theme="filled"
                      class="question"
                    />
                  </div>
                </a-popover>
              </span>
            </a-table-column>
          </a-table>
          <a-pagination
            class="pagination"
            :total="85"
            :show-total="total => `共${total}条，每页20条`"
            :page-size="20"
            :default-current="1"
          />
        </a-tab-pane>
      </a-tabs>
    </div>
  </div>
  <!-- </a-spin> -->
</template>

<script>
export default {
  data() {
    return {
      branch_office: [
        {
          id: 1,
          value: '刘德华'
        },
        {
          id: 2,
          value: '张学友'
        },
        {
          id: 3,
          value: '迪丽热巴'
        }
      ],
      // 概况表单的数据绑定对象
      GeneralSituationForm: {
        teacher: '',
        date: ''
      },
      // 概况数据列表
      GeneralSituationList: [],
      // 总数据列表
      total: 0,
      // 查询条件
      querInfo: {},
      GeneralSituationFormRef: {}
    }
  },
  created() {
    this.getGeneralSituationList()
  },
  methods: {
    handleChange(value) {
      console.log(`selected ${value}`)
    },
    // 获取学员统计报表--概况数据
    async getGeneralSituationList() {
      const { data: res } = await this.$http.get('xxx', {})
      if (res.meta.status !== 200) {
        this.$message.error('获取商品分类失败')
      }
      console.log(res.data)
      // 把概况数据列表，赋值给 GeneralSituationList
      // this.GeneralSituationList = res.data.result
      // 为总数据条数赋值
      // this.total = res.data.total
    },
    // 概况搜索
    // generalSituationSearch(e) {
    //   console.log('概况搜索')
    // },
    // 概况导出
    generalSituationExport() {
      console.log('概况导出')
    },
    // 明细搜索
    detailSeacrh() {
      console.log('明细搜索')
    },
    // 明细导出
    detailExport() {
      console.log('明细导出')
    },
    // 清空搜索条件
    resetForm(formName) {
      // console.log(this)
      // if (this.$refs[formName] !== undefined) {
      this.$refs[formName].resetFields()
      // }
    },
    // 搜索按钮获取值
    handleSubmit(e) {
      e.preventDefault()
      this.form.validateFieldsAndScroll((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values)
        }
      })
    },
    // 监听 pagesize 改变
    // handleSizeChange(newSize){
    //   this.
    // }
    // 监听 页数 改变
    handleCurrentChange(newPage) {
      this.querInfo.pagenum = newPage
      this.getGeneralSituationList()
    }
  }
}
</script>

<style lang="less" scoped>
.container {
  position: relative;
}
.background {
  padding: 20px;
  background-color: #fff;
  border-radius: 7px;
  min-width: 1400px;
}
.title {
  color: #190101;
  margin-top: 20px;
  font-size: 15px;
  margin-bottom: 5px;
}
.inline {
  display: inline;
}
.question {
  color: #999;
}
.width-22 {
  width: 22%;
}
.width-160 {
  margin-left: 10px;
  width: 200px;
}
.marginTop-20 {
  margin-top: 20px;
}
.question {
  margin-left: 3px;
}
.check-span-clear {
  color: #2692fc;
  margin-left: 20px;
  cursor: pointer;
}
.button-group {
  display: inline-block;
  line-height: 40px;
}
.search_group {
  width: 0% 300px;
  margin-right: 20px;
}
.tab-width-70 {
  display: inline-block;
  width: 70px;
  text-align: center;
}
.pagination {
  display: flex;
  margin-top: 10px;
  justify-content: flex-end;
}
</style>
