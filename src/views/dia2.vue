<template>
    <div class="dia order">
        <div class="main ">

            <div class="goods" style="margin-bottom: 30px">
                <div class="diatitle title">
                    <ul>
                        <li>商品图片</li>
                        <li>商品名</li>
                        <li>商品单价</li>
                        <li>商品数量</li>
                        <li>商品总价</li>
                        <li>退款</li>

                    </ul>
                </div>
                <div class="content" v-for="(goods,index) in goodsData" :key="index">
                    <ul>
                        <li><img :src="url+goods.pic_url" alt=""></li>
                        <li>{{goods.goods_name}}</li>
                        <li>￥{{goods.goods_price}}</li>
                        <li>{{goods.number}}</li>
                        <li>￥{{goods.zprice}}</li>
                        <li>   <el-button type="success" @click="tuikuan(goods.attr_id,goods.orders_id)" :plain="true" v-if="goods.is_refund==1" style="text-align: center">
                            退款
                        </el-button>
                        <div v-if="goods.is_refund==2">
                            已退款
                        </div>
                        </li>
                    </ul>
                </div>

            </div>
            <div style="overflow: hidden;border-bottom: 1px solid gainsboro">
                <div class="fl dia"><span class="dialogTitile">发货地址:</span><span class="txt"> {{goodsData[0].address_info}}</span></div>
                <div class="fr dia"><span class="dialogTitile">收件人:</span><span  class="txt"> {{ goodsData[0].order_name}}</span></div>
                <div class="fl dia"><span class="dialogTitile">联系方式:</span><span  class="txt"> {{goodsData[0].order_tel}}</span></div>
                <div class="fr dia"><span class="dialogTitile">物流公司:</span><span  class="txt"> {{goodsData[0].log_name}}</span></div>
                <div class="fl dia"><span class="dialogTitile">物流编号:</span><span  class="txt">  {{goodsData[0].log_number}}</span></div>
            </div>
            <div style="overflow: hidden; padding-top: 20px;padding-bottom: 40px">
                <div class="fl dia"><span class="dialogTitile">订单编号:</span><span class="txt"> {{goodsData[0].orders_no}}</span></div>
                <div class="fr dia"><span class="dialogTitile">下单时间:</span><span  class="txt"> {{goodsData[0].orders_ctime}} </span></div>
                <div class="fl dia"><span class="dialogTitile">支付流水号:</span><span  class="txt"> {{goodsData[0].orders_number}}</span></div>
                <div class="fr dia"><span class="dialogTitile">发货时间:</span><span  class="txt"> {{goodsData[0].begin_time}}</span></div>
                <div class="fl dia"><span class="dialogTitile">支付方式:</span><span  class="txt"> {{ goodsData[0].orders_pay=='0'?'支付宝':'积分支付'}}</span></div>
                <div class="fr dia"><span class="dialogTitile">收货时间:</span><span  class="txt"> {{goodsData[0].end_time}}</span></div>
                <div class="fl dia"><span class="dialogTitile">订单状态:</span><span  class="txt"> {{goodsData[0].orders_state}}</span></div>
                <div class="fr dia"><span class="dialogTitile">订单备注:</span><span  class="txt"> {{goodsData[0].order_remark}}</span></div>

            </div>
            <el-button type="info" @click="back">返回</el-button>
        </div>
    </div>
</template>
<style scoped>
    .diatitle{
        padding-left: 0;
    }
    .goods .content ul li{
        vertical-align: bottom;
    }
    li img{
        width: 100px;
        height: 100px;
    }
    .order{
        width: 1170px;
        background: #ffffff;
    }
    .main{
        width: 1000px;
        margin: 0 auto ;
        padding-top: 30px;
        padding-left: 15px;
    }
    .dialogTitile{
        font-weight: bolder;
        font-size: 14px;
    }
    .fr{
        float: right;
    }
    .fl{
        float: left;
    }
    .dia{
        min-width: 400px;
        text-align: left;
        padding-bottom: 10px;
    }
    .txt{
        font-size: 12px;
        color: #888888
    }
    .goods  .title ul li {
        background: black;
        color: white;
        display: inline-block;
        border-right: 1px solid white;
        font-size: 12px;
        text-align: center;
        line-height: 30px;
        height: 30px;
    }
    .goods  .title ul li:nth-child(1){
        width: 230px;

    }
    .goods  .title ul li:nth-child(2){
        width: 230px;
    }
    .goods  .title ul li:nth-child(3){
        width: 130px;
    }
    .goods  .title ul li:nth-child(4){
        width: 130px;
    }
    .goods  .title ul li:nth-child(5){
        width: 130px;
    }
    .goods  .title ul li:nth-child(6){
        width: 130px;
    }
    .goods  .content ul li:nth-child(1){
        width: 230px;

    }
    .goods  .content ul li:nth-child(2){
        width: 230px;
    }
    .goods  .content ul li:nth-child(3){
        width: 130px;
    }
    .goods  .content ul li:nth-child(4){
        width: 130px;
    }
    .goods  .content ul li:nth-child(5){
        width: 130px;
    }
    .goods  .content ul li:nth-child(6){
        width: 130px;
    }
    .goods  .title ul{
        margin-bottom: 30px;
    }
    .goods  .content ul li {
        padding: 5px 40px ;
        display: inline-block;
        border: 1px solid gainsboro;
        font-size: 12px;
        height: 100px;
        line-height: 100px;
        text-align: center;
    }
    .goods  .content ul{
    }
</style>
<script>
    export default {
        name:'dia',
        data() {
            return {
                id:'',
                diaDate:{},
                goodsData:[{}],
                url:'',
                uuid:''
            }
        },
        created(){
            this.uuid=JSON.parse(localStorage.getItem('userinfo')).uuid
            this.url=localStorage.getItem('URL')
            this.id = sessionStorage.getItem("id");
            console.log(this.id);
            this.$http.get('/manage/showGoodsInfoInOrders.do',{
                params:{
                    uuid:this.uuid,
                    ordersId:this.id
                }
            })
                .then((res)=>{

                    console.log(res.data.data);
                    // this.diaDate=res.data.data[0];
                    this.goodsData=res.data.data;
                    for(let i=0;i<this.goodsData.length;i++){
                        let zprice=this.goodsData[i].number*this.goodsData[i].goods_price;
                        this.goodsData[i].zprice=zprice
                    }
                    // console.log(this.goodsData);
                    // this.goodsData=res.data.list[0]
                    // console.log(res.data.data.splice(0,res.data.data.length-1));
                    // this.page=res.data.data.splice(res.data.data.length-1,1)[0];
                    // this.currentPage=this.page.currentPage;
                    // this.pageCount=this.page.totalPage;
                    // console.log(this.currentPage);
                    // console.log(this.pageCount);
                    // this.tableData=res.data.data;
                    // this.searchData=this.tableData;
                    // console.log(this.tableData);
                })
                .catch((err)=>{
                    return console.log(err);
                })
        },
        methods: {
            back(){
                sessionStorage.clear();
                this.$router.push('/HOrder')
            },

            tuikuan(i,a){

                console.log(i,a);
                this.$http.get('/manage/changeRefundState.do',{
                    params:{
                        uuid:this.uuid,
                        attrId:i,
                        ordersId:a
                    }
                })
                .then((res)=>{
                    console.log(res);
                    let that=this;
                    this.$message('退款成功');
                    setTimeout(function () {
                        that.$router.push('/HOrder')
                    },4000)
                })
                .catch((err)=>{
                    return console.log(err);
                })
            }
        }
    }
</script>