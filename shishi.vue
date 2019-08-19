
<template>
  <div class="nodata-container">
    <p>暂无数据{{monthChange}}---{{days}}</p>
    <p>{{currentDateStr}}</p>
    <button @click="targChange('up')">上一月</button>
    <select v-model="monthChange">
      <option v-for="(item,index) in month" :key="index" :value="index + 1">{{index+1}}月</option>
    </select>
    <button @click="targChange('down')">下一月</button>
    <div class="table">
      <ul class="ul_th clearfix">
        <li class="li" v-for="item in myweek" :key="item">{{item}}</li>
      </ul>
      <ul class="clearfix">
            <li class="li" :class="item.bg=='hui'?'hui':''" v-for="(item,index) in csws" :key="index">{{item.day}}</li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  name: "shishi", // Do not use built-in or reserved HTML elements as component id
  data() {
    return {
      needDisplay: false,
      date: [],
      year: "2019", //年
      month: 12,
      monthChange: new Date().getMonth() + 1,
      days: [],
      csws:[],
      myweek: ["周日","周一", "周二", "周三", "周四", "周五", "周六"],

      current: {} // 当前时间: false,
    };
  },
  watch: {
    monthChange: {
      handler(newValue, oldValue) {
          this.current.month = newValue;
        this.csws = [];
            this.forDays();
      }
    }
  },
  computed: {
    // 显示当前时间
    currentDateStr() {
      let { year, month } = this.current;
      return `${year}年${this.pad(month)}月`;
    }
  },
  mounted() {
    this.init();
  },
  methods: {
    targChange(e) {
      if (e == "up") {
        this.monthChange =
          this.monthChange <= 1 ? this.monthChange : this.monthChange - 1;
      } else {
        this.monthChange =
          this.monthChange >= 12 ? this.monthChange : this.monthChange + 1;
      }
    },
    init() {
      this.setCurrent();
      this.forDays();
    },
    forDays(d = new Date(), current = {}){
        let upM=[],
        centerM=[],
          downM=[];
        // 上一个月
        for(let i=this.getDays('1');0<i;i--){
            upM.push({day:new Date(this.current.year, this.current.month<2?this.current.month:(this.current.month -1 ), 0).getDate()-i,bg:'hui'})//new Date(this.current.year, this.current.month<2?this.current.month:(this.current.month -1 ), 0).getDate()-i
        }
        // 当前月
        let arr = new Date(this.current.year,this.current.month,0).getDate();
         for (let i = 0; i < arr; i++) {
          centerM.push({day:i+1,bg:'liang'});//{day:i+1,bg:'liang'}
        }
        // 下一个月  this.getDays(centerM[centerM.length-1].day)
        for(let i=0;i<6-this.getDays(centerM[centerM.length-1].day);i++){
          console.log('jzg=====',this.getDays(centerM[centerM.length-1].day))
          if(this.getDays(centerM[centerM.length-1].day)<6){
            downM.push({day:i+1,bg:'hui'})
          }
        }
        this.csws= [...upM,...centerM,...downM]

          console.log(`上个月=${this.current.month<2?this.current.month:(this.current.month -1 )}号===`,  upM);
          console.log(`当前月=${this.current.month}号===`,  centerM);
          console.log('upok=====',upM)//arr.slice(0,1)
          console.log('centerM=====',centerM)//arr.slice(0,1)
          console.log('downok=====',downM)//arr.slice(0,1)
          console.log('zongde=====',this.csws)//arr.slice(0,1)
          console.log(`下个月=${this.current.month>11?this.current.month :(this.current.month+ 1)}号===`,  downM);
    },
    //获取指定日期的是周几
    getDays(hao){
       return new Date(`${this.current.year}/${this.current.month}/${hao}`).getDay();
    },
    setCurrent(d = new Date(), current = {}) {
      current.year = d.getFullYear();
      current.month = d.getMonth()+1;
      current.date = d.getDate();
      this.current = current;
    },
    pad(e) {
      return Number(e) < 10 ? "0" + e : e;
    },
    Upper(value) {
      console.log("dsdsd===", value);
      let str = new Date(
        this.year + "/" + this.monthChange + "/" + (value + 1)
      ).getDay();
      return str;
    }
  }
};
</script>

<style lang="scss">
@import "./shishi.scss";
</style>