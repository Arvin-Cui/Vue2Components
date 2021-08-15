<template>
  <div class="app-container">
        <Table :columns="columns" :data-source='data' >
          <template v-slot:action='row'>
             <a href="#" style="color:#1890ff" @click="handledelete(row)">Delete</a>
          </template>
        </Table>
  </div>
</template>

<script>
import Table from '@/components/Table/table.vue'
const columns = [
  {
    title: 'Name',
    dataIndex: 'name',
    render:(h,params)=>{
           const str = params.row.name+"处理"
            return h('span',{style:'background:#f0f0f1'}, str)
    },
    key: 'name',
  },
  {
    title: 'Age',
    dataIndex: 'age',
    // customRender:(record)=>this.customRender(record),
    key: 'age',
  },
  {
    title: 'Address',
    dataIndex: 'address',
    key: 'address 1',
  },
  {
    title: 'Long Column Long Column Long Column',
    dataIndex: 'address',
    key: 'address 2',
  },
  {
    title: 'Long Column Long Column',
    dataIndex: 'address',
    key: 'address 3',
  },
  {
    title: 'Action',
    dataIndex: 'action',
    key: 'action',
  },
];
const data = [
  {
    key: '1',
    name: 'John Brown',
    age: 32,
    address: 'New York No. 1 Lake Park, New York No. 1 Lake Park',
    tags: ['nice', 'developer'],
  },
  {
    key: '2',
    name: 'Jim Green',
    age: 42,
    address: 'London No. 2 Lake Park, London No. 2 Lake Park',
    tags: ['loser'],
  },
  {
    key: '3',
    name: 'Joe Black',
    age: 32,
    address: 'Sidney No. 1 Lake Park, Sidney No. 1 Lake Park',
    tags: ['cool', 'teacher'],
  },
];
export default {
  components:{
    Table
  },
  data(){
    return {
      columns,
      data
    }
  },
  methods:{
    customRender(record){
      console.log(record)
    },
    handledelete(row){
      console.log(row)
    }
  }

}
</script>
