<template>
  <div>
    <a-button @click="addColumn">添加列</a-button>
    <a-button @click="addRow">添加行</a-button>
    <a-table v-if="tableShow" :pagination="false" :data-source="data" :columns="columns">
      <template #name="{ column, text, record }">
        <div class="editable-cell">
          <div v-if="editStatus[column.dataIndex]" class="editable-cell-input-wrapper">
            <a-input v-model:value="record[column.dataIndex]" @pressEnter="save(column.dataIndex)" />
          </div>
          <div v-else class="editable-cell-text-wrapper">
            <span>{{ text || ' ' }}</span>
          </div>
        </div>
      </template>
    </a-table>
    <div id="optMenu"></div>
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
      columnNames: [],
    }
  },
  created() {
    console.log('created');
  },
  methods: {
    addColumn() {
      let columnName = `columns${this.columnIndex++}`;
      this.editStatus[columnName] = false;
      this.columns.push({
        title: () => { return this.renderTitle(columnName, columnName) },
        align: 'center',
        dataIndex: columnName,
        // customHeaderCell: this.headerCellRender
      });
      // if (this.data.length > 0) {
      //   this.data[0][columnName] = columnName;
      // }
      // // console.log(this.columns)
      // this.tableShow = false;
      // //此处需要在nextTick里执行才能刷新表格列
      // this.$nextTick(() => {
      //   this.tableShow = true
      // })
    },
    addRow() {
      let row = {};
      this.columns.forEach(item => {
        row[item.dataIndex] = `${item.dataIndex}-row${this.data.length}`
      });
      this.data.push(row);
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
    cellRender(dataIndex, record, rowIndex) {
      console.log(record);
      console.log('dataIndex--------', dataIndex);
      console.log('rowIndex--------', rowIndex);
      if (rowIndex == 0) {
        return {
          on: {
            click: (event) => {
              console.log(event);
            }
          }
        }
      }
    },
    renderTitle(title, dataIndex) {
      // return (
      //   <div>
      //     <span>{title}</span>
      //     <a-input v-model:value={this.columnName} />
      //     <button {...
      //       {
      //         onClick: () => {
      //           console.log(this.columnName);
      //           console.log(dataIndex);
      //         }
      //       }}>
      //       编辑
      //     </button>
      //     <button>
      //       删除
      //     </button>
      //   </div>
      // );
      // <div v-if={editStatus[dataIndex]} class="editable-cell-input-wrapper">
      //       <a-input v-model:value={columnNames[dataIndex]} @pressEnter="save(dataIndex)" />
      //       <a-button v-if="index == 0" @click="save(column.dataIndex)">保存</a-button>
      //     </div>
      //     <div v-else class="editable-cell-text-wrapper">
      //       <span>{{ text || ' ' }}</span>
      //       <a-button v-if="index == 0" @click="edit(column.dataIndex)">编辑</a-button>
      //     </div>
      this.columnNames[dataIndex] = title;
      const vif = () => {
        if(this.editStatus[dataIndex]){
          return (
            <div class="editable-cell-input-wrapper">
            <a-input v-model:value={this.columnNames[dataIndex]}/>
            <a-button {...{onClick:()=>{this.save(dataIndex)}}}>保存</a-button>
            </div>
          )
        }
        else{
          return (
            <div class="editable-cell-input-wrapper">
              <span>{ this.columnNames[dataIndex] || ' ' }</span>
              <a-button {...{onClick:()=>{this.edit(dataIndex)}}}>编辑</a-button>
          </div>
          )
        }
      }
      return (
        <div class="editable-cell">
        {
          vif()
        }
        </div>
      )
    },
    test(value) {
      console.log(value);

      // return value == 0 ? 'table-first-row' : ''
    }
  }
}
</script>
<style>
.table-first-row {
  background-color: #fafafa;
}
</style>