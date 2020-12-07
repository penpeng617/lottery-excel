<template>
    <div>
        <import-excel v-if='flag' @sendLists='sendList'></import-excel>
        <div class="participateNum">参与人数：{{participateNum}}人</div>
        <awards  :list.sync="list" ></awards>
        <canvas-div :list.sync="list" ref="canvasBox" ></canvas-div>
        <winner></winner>
        <div class='positionCenter' ref='awardsEnd'>活动结束!</div>
    </div>
</template>
<script>
import awards from '@/components/awards'
import winner from '@/components/winner'
import canvas from '@/components/canvas'  
import importExcel from '@/components/importExcel'
// import axios from 'axios'
// import mock from '../mock'
// import demoList from '../mock/demoList'
export default {
  name: 'home',
  data () {
    return {
      list:[],
      flag:true,
      participateNum: 0,
      mainCanvas:"",
    }
  },
  components:{
    awards,
    winner,
    'canvas-div':canvas,
    "import-excel":importExcel
  },
  mounted(){
      let _this =this;
      //canvas组件用的watch监听属性，所以必须在此处赋值
      let  list  =JSON.parse(localStorage.getItem('lotteryList')) || [];
      if(list.length >0){
        _this.flag = false;
        _this.list = list;
        _this.participateNum= _this.list.length;
      }else{
        //此处用的是mock自动生成的随机数据，使用时要将 axios mock demoList 组件引入
        // axios.get('/parameter/query').then(res=>{
        //     let data = res.data.data.list;
        //     _this.list = data;
        //      _this.participateNum= _this.list.length;
        //     localStorage.setItem("lotteryList",JSON.stringify(data));
        // })
      }
  },
  updated (){
    this.mainCanvas = document.getElementById('mainCanvas');
  },
  methods:{
    //与上面mock生成的数据选择一种即可，此方法是子组件importExcel导入excel的本地数据，使用时将importExcel 引入
    sendList(data){
      this.list =data;
      this.participateNum= this.list.length;
      localStorage.setItem("lotteryList",JSON.stringify(data));
      this.flag = false;
    }
  }
}
</script>
<style scoped>
 @import '../assets/css/com.css';
.participateNum{top:1%;right:5%;height:50px;line-height:50px;width:200px;border-radius:30px;}
.positionCenter{display:none;background:#fff;color:red;font-weight:bolder;letter-spacing: 6px;font-size:32px;width:300px;height:100px;line-height:100px;text-align:center;border-radius: 10px;}

</style>