
<template>
  <div class="nodata-container">
    <p>暂无数据{{monthChange | bulin}}---{{days}}</p>
    <p>{{currentDateStr}}</p>
    <button @click="targChange('up')">上一月</button>
    <select v-model="monthChange">
      <option v-for="(item,index) in month" :key="index" :value="index + 1">{{index+1 | bulin}}月</option>
    </select>
    <button @click="targChange('down')">下一月</button>
    <div class="table">
      <ul class="ul_th clearfix">
        <li class="li" v-for="item in myweek" :key="item">{{item}}</li>
      </ul>
      <ul class="clearfix">
            <li class="li" :class="item.bg=='hui'?'hui':''" v-for="(item,index) in ganCsws" :key="index">
              {{item.day}}
              <span v-if="item.month+'-'+item.day == items.legalDate" v-for="(items,indexs) in jieri">{{items.name}}</span>
            </li>
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
      myweek: ["周一", "周二", "周三", "周四", "周五", "周六","周日"],
      jieri:[
        {
          legalDate: "08-15",
          name: "中"
        },{
          legalDate: "08-01",
          name: "建"
        }
      ],
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
  filters: {
      bulin(value) {
          console.log('bulin=====',value<10?'0'+value:value+'')
　　　　　　return value<10?'0'+value:value
　　　　}
　　},
  computed: {
    // 显示当前时间
    currentDateStr() {
      let { year, month } = this.current;
      return `${year}年${this.pad(month)}月`;
    },
    ganCsws() {
      for (let i = 0; i < this.csws.length; i++) {
        if (this.csws[i].day < 10) {
          this.csws[i].day = `0${this.csws[i].day}`
        }
        if(this.csws[i].month < 10 ){
          this.csws[i].month = `0${this.csws[i].month}`
        }
      }
      return this.csws
    },
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
          if(this.getDays('1')==0){
            // 如果当前日期为周日的话  这个循环代表前面有几个位置 因为循环是从0开始的 0,1,2,3,4,5长度是6
            for(let j = 5;0<=j;j--){
              upM.push({month:this.monthChange-1,day:new Date(this.current.year, this.current.month<2?this.current.month:(this.current.month -1 ), 0).getDate()-j,bg:'hui'})
                console.log('j====',j);
            }
          }else{
            // 上一个月  这个循环代表前面有几个位置 因为循环是从0开始的
            for(let i=this.getDays('1')-2;0<=i;i--){
                console.log('i====',i);
                upM.push({month:this.monthChange-1,day:new Date(this.current.year, this.current.month<2?this.current.month:(this.current.month -1 ), 0).getDate()-i,bg:'hui'})//new Date(this.current.year, this.current.month<2?this.current.month:(this.current.month -1 ), 0).getDate()-i
            }
          }
          
            
        // 当前月
        let arr =this.getDays('0','getDate')
          console.log('当月总天数====',new Date(this.current.year, this.current.month<2?this.current.month:(this.current.month -1 ), 0).getDate())
         for (let i = 0; i < arr; i++) {
          centerM.push({month:this.monthChange,day:i+1,bg:'liang'});//{day:i+1,bg:'liang'}
        }
        // 下一个月  this.getDays(centerM[centerM.length-1].day)
          for(let i=0;i<7-this.getDays(centerM[centerM.length-1].day);i++){
            console.log('jzg=====',this.getDays(centerM[centerM.length-1].day))
            if(this.getDays(centerM[centerM.length-1].day)<7){
              downM.push({month:this.monthChange+1,day:i+1,bg:'hui'})
            }
        }
        
        this.csws= [...upM,...centerM,...downM]
    },
    //获取指定日期的是周几
    getDays(hao,fun='getDay'){
        switch (fun){
          case 'getDate'://获取当月有多少天
            return new Date(this.current.year,this.current.month,hao)[fun]();
          break;
          default://获取对应天是周几
             return new Date(`${this.current.year}/${this.current.month}/${hao}`)[fun]();
        }
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