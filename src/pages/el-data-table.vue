<template>
  <el-data-table
    ref="table"
    v-bind="tableConfig"
    :tableAttrs="tableAttrs"
    :formAttrs="formAttrs"
  >
    <el-table-column label="操作">
      <template
        slot="default"
        slot-scope="scope"
      >
        <el-button
          type="primary"
          size="small"
          @click="onDefaultView(scope.row)"
        >
          查看
        </el-button>
        <el-button
          size="small"
          @click="onDefaultEdit(scope.row)"
        >
          修改
        </el-button>
        <el-button
          size="small"
          @click="onCustomOperation(scope.row)"
        >
          {{scope.row.status === 0 ? '上架' : '下架'}}
        </el-button>
      </template>
    </el-table-column>
  </el-data-table>
</template>

<script>
  import moment from 'moment';

  export default {
    data() {
      return {
        tableConfig: {
          url: 'https://www.easy-mock.com/mock/5c7fdead8cfae820245a9b41/api/v1/users',
          hasView: false,
          hasEdit: false,
          hasOperation: false,
          dialogEditTitle: '编辑组件',
          dialogNewTitle: '新增组件',
          dialogViewTitle: '查看组件',
          columns: [
            {
              type: 'selection',
              selectable: (row, index) => true,
            },
            {
              prop: 'name',
              label: '组件名称',
            },
            {
              prop: 'category',
              label: '分类',
            },
            {
              prop: 'version',
              label: '版本',
            },
            {
              prop: 'language',
              label: '开发语言',
            },
            {
              prop: 'last',
              label: '最后更新时间',
              formatter: row => moment(row.last, 'x')
                .format('YYYY-MM-DD'),
            },
            {
              prop: 'status',
              label: '状态',
              formatter: row => (row.status === 0 ? '下架' : '上架'),
            },
          ],
          searchForm: [
            {
              $el: { placeholder: '请输入' },
              label: '组件名称',
              $id: 'name',
              $type: 'input',
            },
            {
              $el: { placeholder: '请选择' },
              label: '分类',
              $id: 'category',
              $type: 'select',
              $options: ['前端组件', '测试子组件', '分布式工具', '应用服务', '数据存储'].map(f => ({
                label: f,
                value: f,
              })),
            },
            {
              $el: { placeholder: '请选择' },
              label: '状态',
              $id: 'status',
              $type: 'select',
              $options: [0, 1].map(f => ({
                label: f === 0 ? '下架' : '上架',
                value: f,
              })),
            },
          ],
          form: [
            {
              $type: 'input',
              $id: 'name',
              label: '组件名称',
              $el: {
                placeholder: '请输入组件名称',
                width: '100%',
              },
              rules: [
                {
                  required: true,
                  message: '请输入组件名称',
                  trigger: 'blur',
                },
              ],
            },
            {
              $el: { placeholder: '请选择' },
              label: '组件分类',
              $id: 'category',
              $type: 'select',
              $options: ['前端组件', '测试子组件', '分布式工具', '应用服务', '数据存储'].map(f => ({
                label: f,
                value: f,
              })),
              rules: [
                {
                  required: true,
                  message: '请输入组件分类',
                  trigger: 'blur',
                },
              ],
            },
            {
              $type: 'input',
              $id: 'author',
              label: '作者',
              $el: {
                placeholder: '作者',
              },
              rules: [
                {
                  required: true,
                  message: '请输入作者名称',
                  trigger: 'blur',
                },
              ],
            },
            {
              $id: 'version',
              $type: 'input',
              label: '版本号',
              $el: {
                placeholder: '版本号',
              },
              rules: [
                {
                  required: true,
                  message: '请输入版本号',
                  trigger: 'blur',
                },
              ],
            },
            {
              $id: 'language',
              $type: 'select',
              label: '开发语言',
              $el: {
                placeholder: '请选择',
              },
              $options: ['JavaScript', 'Java'].map(f => ({
                label: f,
                value: f,
              })),
              rules: [
                {
                  required: true,
                  message: '请选择开发语言',
                  trigger: 'blur',
                },
              ],
            },
            {
              $id: 'summary',
              $type: 'input',
              label: '简介',
              $el: {
                placeholder: '请输入简介URL',
              },
            },
            {
              $id: 'api',
              $type: 'input',
              label: '接口说明',
              $el: {
                placeholder: '请输入接口说明URL',
              },
            },
            {
              $id: 'history',
              $type: 'input',
              label: '版本记录',
              $el: {
                placeholder: '请输入版本记录URL',
              },
            },
            {
              $id: 'guide',
              $type: 'input',
              label: '快速入门',
              $el: {
                placeholder: '请输入快速入门URL',
              },
            },
            {
              $id: 'example',
              $type: 'input',
              label: '示例',
              $el: {
                placeholder: '请输入示例URL',
              },
            },
            {
              $id: 'icon',
              $type: 'input',
              label: '组件图标',
              $el: {
                placeholder: '请输入组件图标',
              },
              rules: [
                {
                  required: true,
                  message: '请输入组件图标',
                  trigger: 'blur',
                },
              ],
            },
            {
              $id: 'desc',
              $type: 'input',
              label: '组件说明',
              $el: {
                placeholder: '请输入组件说明',
              },
              rules: [
                {
                  required: true,
                  message: '请输入组件说明',
                  trigger: 'blur',
                },
              ],
            },
          ],
          extraButtons: [
            {
              type: 'primary',
              text: '查看',
              atClick: row => {
                this.$refs['table'].onDefaultView(row);
                return Promise.resolve();
              },
            },
            {
              type: '',
              text: '编辑',
              atClick: row => {
                this.$refs['table'].onDefaultEdit(row);
                return Promise.resolve();
              },
            },
            {
              type: '',
              text: '上架',
              atClick: row => {
                return Promise.resolve();
              },
            },
          ],
        },
        tableAttrs: {
          'cell-class-name': this.cellClass,
        },
        formAttrs: {
          'label-position': 'right',
          'label-width': '80px',
        },
      };
    },
    methods: {
      cellClass({ row, column, rowIndex, columnIndex }) {
        if (columnIndex !== 6) {
          return 'gray';
        }

        if (row.status === 0) {
          return 'gray';
        }
        return 'green';
      },
      onDefaultView(row){
        this.$refs['table'].onDefaultView(row);
      },
      onDefaultEdit(row){
        this.$refs['table'].onDefaultEdit(row);
      },
      onCustomOperation(row) {

      },
    },
  };
</script>

<style lang="stylus">
  .gray {
    color: gray
  }

  .green {
    color: green
  }
</style>
