<template>
  <a-table :rowKey="(record, index) => record.key" :row-selection="rowSelection" :columns="columns" :data-source="data" :pagination="pagination">
    <template #bodyCell="{ column, text }">
      <template v-if="column.dataIndex === 'name'">
        <a>{{ text }}</a>
      </template>
    </template>
  </a-table>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  data() {
    return {
      columns: [
        {
          title: 'Name',
          dataIndex: 'name',
        }, 
        {
          title: 'Age',
          dataIndex: 'age',
        }, 
        {
          title: 'Address',
          dataIndex: 'address',
        }
      ],
      data: [
        {
          key: '1',
          name: 'John Brown',
          age: 32,
          address: 'New York No. 1 Lake Park',
        }, 
        {
          key: '2',
          name: 'Jim Green',
          age: 42,
          address: 'London No. 1 Lake Park',
        }, 
        {
          key: '3',
          name: 'Joe Black',
          age: 32,
          address: 'Sidney No. 1 Lake Park',
        }, 
        {
          key: '4',
          name: 'Disabled User',
          age: 99,
          address: 'Sidney No. 1 Lake Park',
        },
        {
          key: '5',
          name: 'John Brown',
          age: 32,
          address: 'New York No. 1 Lake Park',
        }, 
        {
          key: '6',
          name: 'Jim Green',
          age: 42,
          address: 'London No. 1 Lake Park',
        }, 
        {
          key: '7',
          name: 'Joe Black',
          age: 32,
          address: 'Sidney No. 1 Lake Park',
        }, 
        {
          key: '8',
          name: 'Disabled User',
          age: 99,
          address: 'Sidney No. 1 Lake Park',
        },
        {
          key: '9',
          name: 'John Brown',
          age: 32,
          address: 'New York No. 1 Lake Park',
        }, 
        {
          key: '10',
          name: 'Jim Green',
          age: 42,
          address: 'London No. 1 Lake Park',
        }, 
        {
          key: '11',
          name: 'Joe Black',
          age: 32,
          address: 'Sidney No. 1 Lake Park',
        }, 
        {
          key: '12',
          name: 'Disabled User',
          age: 99,
          address: 'Sidney No. 1 Lake Park',
        },
        {
          key: '13',
          name: 'John Brown',
          age: 32,
          address: 'New York No. 1 Lake Park',
        }, 
        {
          key: '14',
          name: 'Jim Green',
          age: 42,
          address: 'London No. 1 Lake Park',
        }, 
        {
          key: '15',
          name: 'Joe Black',
          age: 32,
          address: 'Sidney No. 1 Lake Park',
        }, 
        {
          key: '16',
          name: 'Disabled User',
          age: 99,
          address: 'Sidney No. 1 Lake Park',
        }
      ],
      selectedRowKeys: [],
      isValid: true,
      pagination: {
        current: 1,
        pageSize: 5,
        onChange: this.handlePageChange,
      },
      selectedRowKeys: [],
      selectedRow: [],
    };
  },

  computed: {
    rowSelection() {
      return {
        selectedRowKeys: this.selectedRowKeys,
        onChange: this.onRowSelectionChange,
        getCheckboxProps: (record) => ({
          disabled: record.name === 'Disabled User',
        }),
        onSelectAll: (selected) => {
          if (selected) {
            if (!this.isValid) {
              this.selectedRowKeys = this.data.map((row) => row.key);
            } else {
              const keyArr = [];
              for (let index = 0; index < this.data.length; index++) {
                const element = this.data[index];
                if (element.name !== 'Disabled User') {
                  keyArr.push(element.key);
                }
              }
              this.selectedRowKeys = keyArr;
            }
          } else {
            this.selectedRowKeys = [];
          }
        },
      };
    }
  },

  methods: {
    handlePageChange(page, pageSize) {
      this.pagination.current = page;
      this.pagination.pageSize = pageSize;
    }
  }
});
</script>
