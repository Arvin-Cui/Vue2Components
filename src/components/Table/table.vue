<template>
  <div class="table">
    <div class="table_tr">
      <div class="table_th" v-for="(item, index) in columns" :key="item.key">
        {{ item.title }}
      </div>
    </div>
    <div class="table_tr2" v-for="(itm, ind) in dataSource" :key="itm.key">
      <div class="table_td" v-for="(item, index) in columns">
        <template v-if="item.render" >
             <Render :column="item" :row="itm" :index="ind" :render="item.render"></Render>
        </template>
       
        <template v-else>
                 {{itm[item.dataIndex]}}
          </template>
         <template v-if="item.dataIndex==='action'" >
             <slot name='action' v-bind:row='itm'></slot>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import Render from './table'
export default {
  props: {
    columns: {
      type: Array,
      deafult: () => {
        return [];
      },
    },
    dataSource: {
      type: Array,
      deafult: () => {
        return [];
      },
    },
  },
  components:{
      Render
  }
};
</script>

<style lang="scss" scoped>
.table {
  width: 100%;
  display: table;
  border-collapse: collapse;
  table-layout: fixed;
  .table_tr {
    display: table-row;
    height: 50px;
    background-color: #fafafa;
  }
  .table_th {
    display: table-cell;
    border: 1px solid #e4e4e4;
    text-align: center;
    vertical-align: middle;
    font-size: 12px;
    font-family: PingFangSC-Semibold, PingFang SC;
    font-weight: 600;
    color: #2b3530;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
     padding: 0 10px;
  }
  .table_tr2 {
    display: table-row;
    height: 50px;
    border: 1px solid #e4e4e4;
  }
  .table_tr2:hover{
      background-color: #e6f7ff;
  }
  .table_td {
    display: table-cell;
    border: 1px solid #e4e4e4;
    text-align: center;
    vertical-align: middle;
    font-size: 12px;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #2b3530;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    padding: 0 10px;
  }
}
</style>