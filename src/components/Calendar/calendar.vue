<template>
  <div class="calendarbox">
    <el-row>
      <el-col :span="12">
        <el-date-picker
          v-model="screendate"
          type="month"
          placeholder="请选择选择月份"
          @change="screendatechange"
          :clearable="false"
        >
        </el-date-picker>
      </el-col>
      <el-col :span="12" >
        <div class="right">
        <el-button v-if="checked" type="primary" style="margin-right: 60px" @click="submit">提交</el-button>
        <el-checkbox v-model="checked" @change="checkedchange">多选</el-checkbox>
      </div
      ></el-col>
    </el-row>
    <el-row>
      <el-col :span="12" :offset="17" >
        <div class="tipbox">
            <div v-for="(item,index) in tip" :key="index" class="tipboxchild">
                <span :class="index===(tip.length-1)?'firbox':'sebox'" :style="[index===0 && {background:'#ffffff'}]">{{index===(tip.length-1)?"5":""}}</span>
                <span class="tiptitle">{{item.title}}</span>
            </div>
        </div>
      </el-col>
    </el-row>
    <el-row>
      <div class="top">
        <span v-for="(item, index) in week" :key="index">{{ item }}</span>
      </div>
      <div class="contentbox">
        <div v-for="i in 6" :key="i" class="rowbox">
          <div
            v-for="j in 7"
            :key="j"
            class="columnbox"
            :class="[
              { lastrow: isLastrow(i) },
              { lastcolumn: isLastColumn(j) },
              { week: isWeek(j) },
              {
                notCurrentMonth: !isCurrentMonth(
                  visibleDays[(i - 1) * 7 + (j - 1)].time
                ),
              },
              { today: isToday(visibleDays[(i - 1) * 7 + (j - 1)].time) },
              {active:isActive(visibleDays[(i - 1) * 7 + (j - 1)].active)}
            ]"

            @click="handleSelect(visibleDays[(i - 1) * 7 + (j - 1)],j,)"
          >
            <span class="day">{{
              visibleDays[(i - 1) * 7 + (j - 1)].time.getDate()
            }}</span>
            <div class="noticebox" v-if="!isWeek(j) &&  isCurrentMonth(visibleDays[(i - 1) * 7 + (j - 1)].time)">
                <span :class="visibleDays[(i - 1) * 7 + (j - 1)].am?'free':''">{{visibleDays[(i - 1) * 7 + (j - 1)].am?visibleDays[(i - 1) * 7 + (j - 1)].am.remark:''}}</span>
                <span :class="visibleDays[(i - 1) * 7 + (j - 1)].pm?'free':''">{{visibleDays[(i - 1) * 7 + (j - 1)].pm?visibleDays[(i - 1) * 7 + (j - 1)].pm.remark:''}}</span>
            </div>
          </div>
        </div>
      </div>
    </el-row>
  </div>
</template>
<script>
const getYearMonthDay = (date) => {
  const year = date.getFullYear();
  const month = date.getMonth();
  const day = date.getDay();
  const time = date.getDate();
  return { year, month, day, time };
};
const getDate = (year, month, day = 1) => {
  return new Date(year, month, day);
};
export default {
  data() {
    return {
      currentTime: {
        year: "",
        month: "",
      },
      week: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"],
      screendate: "",
      tip:[{title:"需评时段"},{title:"免评时段"},{title:"法定节假日"} ],//提示信息
      checked:false,//多选
      select:[],//选择时间
      current:"2021-08-01",//时间选择器绑定值
    };
  },
  props:{
    jobList:{
      type:Array,
      default:()=>{
        return []
      }
    },
    dateGroup:{
      type:Array,
      default:()=>{
        return []
      }
    },
  },
  mounted() {
    //获取当前时间
    this.getYearMonth();
  },
  methods: {
    screendatechange() {
      const { year, month } = getYearMonthDay(this.screendate);
      this.currentTime = {
        year,
        month,
      };
      this.$emit('screendatechanged',this.currentTime)
    },
    //获取当前时间
    getYearMonth() {
      var myDate = new Date();
      this.screendate=new Date();
      const { year, month } = getYearMonthDay(myDate);
      this.currentTime = {
        year,
        month,
      };
    },
    isLastrow(i) {
      if (i === 6) {
        return true;
      }
    },
    isLastColumn(j) {
      if (j === 7) {
        return true;
      }
    },
    isWeek(j) {
      if (j === 6 || j === 7) {
        return true;
      }
    },
    isActive(active){
      if(active){
        return true
      }else{
        return false
      }
    },
    //是否是当前月份日期
    isCurrentMonth(date) {
      const { year, month } = getYearMonthDay(
        new Date(this.currentTime.year, this.currentTime.month)
      );
      const { year: years, month: months } = getYearMonthDay(date);
      return year === years && month === months;
    },
    //是否是今日
    isToday(date) {
      const { year, month, time } = getYearMonthDay(new Date());
      const { year: years, month: months, time: times } = getYearMonthDay(date);
      return year === years && month === months && time === times;
    },
    //月份切换
    pervMonth() {
      const d = getDate(this.currentTime.year, this.currentTime.month, 1);
      d.setMonth(--this.currentTime.month);
      let { year, month } = getYearMonthDay(d);
      this.currentTime = {
        year,
        month,
      };
    },
    nextMonth() {
      const d = getDate(this.currentTime.year, this.currentTime.month, 1);
      d.setMonth(++this.currentTime.month);
      let { year, month } = getYearMonthDay(d);
      this.currentTime = {
        year,
        month,
      };
    },
    handleSelect(item,j){
      if(this.checked){
        if(this.isCurrentMonth(item.time) && !this.isWeek(j)  && !item.am && !item.pm){

          if(this.select.length ){
            let bol=true
            this.select.forEach((res,index)=>{
              if(res.time.getTime()===item.time.getTime()){
                this.select.splice(index,1)
                bol=false
              }
            })
            bol && this.select.push(item)
          }else{
            this.select.push(item)
          }
          this.$emit('select',this.select)
        }else{
            // this.$message.error('当前日期不能参与多选')
        }

      }else {
        if(this.isCurrentMonth(item.time) && !this.isWeek(j)){
          this.$emit('selectTime',item,this.checked)
        }

      }
    },
    submit(){
      this.$emit('selectTime',this.select,this.checked)
    },
    //多选切换
    checkedchange(){
      this.select=[]
      this.$emit('select',this.select)
    }
  },
  computed: {
    visibleDays() {
      //获取每个月第一天
      const currentFirstDay = getDate(
        this.currentTime.year,
        this.currentTime.month,
        1
      );
      //计算每个月第一天为周几
      let week = currentFirstDay.getDay();
      //根据第一天是多少 向前推进开始天数 如果是周日 则向前推6天
      week = week === 0 ? 7 : week;
      const startDay = currentFirstDay - (week - 1) * 1000 * 60 * 60 * 24;
      //循环42天
      const arr = [];
      for (let i = 0; i < 42; i++) {
        let obj={}
          this.jobList.forEach((res)=>{
            let dateString = res.day;
            let dateArr = dateString.indexOf('-') !== -1 ?
              dateString.split('-') :
              dateString.indexOf('/') !== -1 ?
                dateString.split('/') :
                [];
           let date=(getDate(Number(dateArr[0]),Number(dateArr[1])-1,Number(dateArr[2])))
            if(new Date(startDay + i * 60 * 60 * 1000 * 24).getTime()===date.getTime()){
              if(res.reviewName==='上午眼操'){
                obj.am=res
              }else{
                obj.pm=res
              }
            }
          })
           this.dateGroup.forEach((res)=>{
            if(new Date(startDay + i * 60 * 60 * 1000 * 24).getTime()===res.time.getTime()){
               obj.active=true
             }})
        obj.time=new Date(startDay + i * 60 * 60 * 1000 * 24)

        arr.push(obj);
      }

      return arr;
    },
  },
};
</script>
<style lang="scss" scoped>
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.calendarbox {
  margin: 50px auto;
  .right{
    height: 40px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }
  .top {
    width: 100%;
    height: 48px;
    display: flex;
    background-color: #F0F0F0;
    color: #333333;
    span {
      display: block;
      width: 14.29%;
      text-align: center;
      height: 48px;
      line-height:48px;
      font-family: PingFangSC-Regular, sans-serif;
      color: #333;
      font-weight: bolder;
      font-style: normal;
      font-size: 13px;

    }
  }
  .contentbox {
    width: 100%;
    font-family: PingFangSC-Regular, sans-serif;
    .rowbox {
      width: 100%;
      height: 100px;
      display: flex;
    }
    .columnbox {
      width: 14.29%;
      height: 100%;
      text-align: center;
      border: 1px solid #e8ecf5;
      border-right: none;
      border-bottom: none;
       color: #333;
       display: flex;
       flex-direction: column;
       align-items: center;
      caret-color: rgba(0,0,0,0);
      cursor: pointer;
    }
    .columnbox:hover{
      background-color: #F5F7FA;
    }
    .day {
      display: block;
      width: 100%;
    }
    .lastrow {
      border-bottom: 1px solid #e8ecf5;
    }
    .lastcolumn {
      border-right: 1px solid #e8ecf5;
    }
    .week {
      background-color: #d7d7d7;
    }
    .week:hover{
      background-color:#d7d7d7 !important ;
    }
    .notCurrentMonth {
      color: #aaaaaa;
    }
    .notCurrentMonth:hover{
      background-color:#fff ;
    }
    .today {
      color: #409eef;
      //background-color: #409eef;
    }
    .active{
      background-color: #409eef !important;
      color: #fff;
    }
    .noticebox{
      display: flex;
      margin:40px auto 0;
      span{
        display: block;
        border-radius: 4px;
        min-width: 50px;
        height: 20px;
        line-height: 20px;
        background-color: #fff;
        font-size: 12px;
        color: #409eef;
      }
      span:nth-child(1){
        border: 1px solid #d9ecff;
      }
      span:nth-child(2){
        border:1px solid #d9ecff;
        border-left:none ;
      }
      .free{
        background-color: #ecf5ff;
        color:#409eff ;
      }
    }
  }
  .tipbox{
    display: flex;
    .tiptitle{
      font-size: 14px;
      color: #7f7f7f;
    }
    .tipboxchild{
      display: flex;
      align-items: center;
    }
    span{
      display: block;
      margin-right: 5px;
      font-family: PingFangSC-Regular, sans-serif;
      font-weight: 400;
      font-style: normal;
      border-radius:3px ;
    }
    .firbox{
      width: 40px;
      height: 23px;
      background-color: #d7d7d7;
      text-align: center;
      line-height: 23px;
      color: #333;
      font-size: 13px;
    }
    .sebox{
      width: 50px;
      height: 20px;
       background-color: #ecf5ff;
       border:1px solid #d9ecff;
    }
  }
}
</style>
