<template>
  <div>
    <a-button @click="addColumn">添加列</a-button>
    <a-button @click="addRow">添加行</a-button>
    <!-- <label>列名称:</label>
    <a-input v-model:value="columnName"></a-input>
    <a-button @click="edit()">编辑</a-button>
    <a-button @click="save()">保存</a-button> -->
    <a-table v-if="tableShow" :pagination="false" :data-source="data" :columns="columns" :showHeader="showHeader"
      :rowClassName="(record, index) => { return test(index) }">
      <template #name="{ column, text, record, index }">
        <div class="editable-cell">
          <div v-if="editStatus[column.dataIndex]" class="editable-cell-input-wrapper">
            <a-input v-model:value="record[column.dataIndex]" @pressEnter="save(column.dataIndex)" />
            <a-button v-if="index == 0" @click="save(column.dataIndex)">保存</a-button>
          </div>
          <div v-else class="editable-cell-text-wrapper">
            <span>{{ text || ' ' }}</span>
            <a-button v-if="index == 0" @click="edit(column.dataIndex)">编辑</a-button>
          </div>
        </div>
      </template>
    </a-table>
  </div>
</template>
  
<script>
export default {
  name: 'TableDemo',
  data() {
    return {
      columns: [],
      data: [],
      showHeader: true,
      tableShow: true,
      columnIndex: 1,
      editStatus: {},
      // columnName: '',
    }
  },
  created() {
    console.log('created');
  },
  methods: {
    addColumn() {
      let columns = `columns${this.columnIndex++}`;
      this.columns.push({
        title: columns,
        align: 'center',
        dataIndex: columns,
        slots: {
          customRender: 'name',
        },
      });
      this.editStatus[columns] = false;
      if(this.data.length > 0){
        this.data[0][columns] = columns;
      }
      // console.log(this.columns)
      this.tableShow = false;
      //此处需要在nextTick里执行才能刷新表格列
      this.$nextTick(() => {
        this.tableShow = true
      })
    },
    addRow() {
      if(this.data.length == 0){
        //隐藏原表头，添加第一行为表头列名称
        this.showHeader = false;
        let firstRow = {};
        this.columns.forEach(item => {
          console.log(item);
          firstRow[item.dataIndex] = item.title;
        })
        this.data.push(firstRow);
      }
      this.data.push({});
    },
    edit(dataIndex) {
      console.log('edit', dataIndex, dataIndex in this.editStatus);
      if (!!dataIndex && dataIndex in this.editStatus) {
        this.editStatus[dataIndex] = true;
      }
    },
    save(dataIndex) {
      if (!!dataIndex && dataIndex in this.editStatus) {
        this.editStatus[dataIndex] = false;
      }
    },
    test(value) {
      console.log(value);
      return value == 0 ? 'table-first-row' : ''
    }
    // titleParam(value) {
    //   console.log(value);
    // },
    // customHeaderRow(column, index) {
    //   console.log('customHeaderRow', column, index);
    // },
    // customHeaderCell(column) {
    //   console.log('customHeaderCell', column);
    // }
  }
}
</script>
<style>
.table-first-row {
  background-color: #fafafa;
}
</style>