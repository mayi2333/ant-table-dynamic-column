<template>
  <div>
    <a-button @click="addColumn">添加列</a-button>
    <a-button @click="addRow">添加行</a-button>
    <label>列名称:</label>
    <a-input v-model:value="columnName"></a-input>
    <a-button @click="edit()">编辑</a-button>
    <a-button @click="save()">保存</a-button>
    <a-table v-if="tableShow" :data-source="data" :columns="columns">
      <template #name="{ column, text, record }">
        <div class="editable-cell">
          <div v-if="editStatus[column.dataIndex]" class="editable-cell-input-wrapper">
            <a-input v-model:value="record[column.dataIndex]" @pressEnter="save(column.dataIndex)" />
            <check-outlined class="editable-cell-icon-check" @click="save(column.dataIndex)" />
          </div>
          <div v-else class="editable-cell-text-wrapper">
            {{ text || ' ' }}
            <!-- <button @click="edit(column)">编辑</button> -->
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
      tableShow: true,
      columnIndex: 1,
      editStatus: {},
      columnName: '',
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
      // console.log(this.columns)
      this.tableShow = false;
      //此处需要在nextTick里执行才能刷新表格列
      this.$nextTick(() => {
        this.tableShow = true
      })
    },
    addRow() {
      this.data.push({});
    },
    edit() {
      console.log('edit', this.columnName, this.columnName in this.editStatus);
      if (!!this.columnName && this.columnName in this.editStatus) {
        this.editStatus[this.columnName] = true;
      }
    },
    save() {
      if (!!this.columnName && this.columnName in this.editStatus) {
        this.editStatus[this.columnName] = false;
      }
    }
  }
}
</script>