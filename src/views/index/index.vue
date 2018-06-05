<template>
    <div class="">
        主页
        <div class="chart" id="chart"></div>
    </div>
</template>

<script>
    //import  from ''
    const echarts = require('echarts');

    export default {
        //组件名
        name: 'index',
        //实例的数据对象
        data() {
            return {

            }
        },
        //数组或对象，用于接收来自父组件的数据
        props: {

        },
        //计算
        computed: {

        },
        //方法
        methods: {

        },
        //生命周期函数 请求写在created中
        created() {

        },
        beforeMount() {

        },
        mounted() {
            const data = {
                "askList": [{ "price": "8.0000000", "volume": "1.0000000000" },{ "price": "8.5000000", "volume": "1.0000000000" },{ "price": "9.9000000", "volume": "1.0000000000" },{ "price": "10.4000000", "volume": "1.0000000000" }, { "price": "11.8000000", "volume": "1.0000000000" },{ "price": "12.0000000", "volume": "1.0000000000" },{ "price": "16.5000000", "volume": "1.0000000000" }, { "price": "18.0000000", "volume": "1.0000000000" }],
                "bidList": [{ "price": "7.0000000", "volume": "1.0000000000" },{ "price": "6.0000000", "volume": "1.0000000000" },{ "price": "5.0000000", "volume": "1.0000000000" }, { "price": "4.1000000", "volume": "1.0000000000" }, { "price": "4.0000000", "volume": "1.0000000000" }, { "price": "3.0000000", "volume": "3.0000000000" }, { "price": "2.0000000", "volume": "1.0000000000" }, { "price": "1.0000000", "volume": "1.0000000000" }]
            };
            //askList 卖 bidList 买
            let { askList, bidList } = data;

            let buyPrice=[],
                sellPrice=[],
                buyTotal=[],
                sellTotal=[]
                ;

            for (const i in bidList) {
                let total = 0;
                for (let n = 0; n <= i; n++) {
                    total += Number(bidList[n].volume);
                }
                buyPrice.push(Number(bidList[i].price))
                buyTotal.push(total);
            }
            buyPrice=buyPrice.reverse();
            buyTotal=buyTotal.reverse();

            for (const i in askList) {
                let total = 0;
                for (let n = i; n < askList.length; n++) {
                    total += Number(askList[n].volume);
                }
                sellPrice.push(Number(askList[i].price))
                sellTotal.push(total);
            }
            console.log(buyPrice,sellPrice,buyTotal,sellTotal)
            let nullList =new Array(buyPrice.length)
            console.log(nullList)
            // 基于准备好的dom，初始化echarts实例
            const myChart = echarts.init(document.getElementById('chart'));
            // 绘制图表
            myChart.setOption({
                legend:{
                    data:['买','卖',],
                    selectedMode:false,
                },
                xAxis: {
                    type: 'category',
                    boundaryGap: false,
                    data: [...buyPrice,...sellPrice]
                },
                yAxis: {
                    type: 'value',
                    splitLine: {
                        show: false
                    },
                    axisLabel: {
                        inside: true,
                        // formatter: '{value}\n'
                    },
                },
                series: [{
                    name:'买',
                    itemStyle: {
                        normal: {
                            color: '#8ec6ad'
                        }
                    },
                    data: buyTotal,
                    symbol:'none',
                    type: 'line',
                    areaStyle: {
                        normal: {
                            color: '#8ec6ad'
                        }
                    },
                },{
                    name:'卖',
                    itemStyle: {
                        normal: {
                            color: 'red'
                        }
                    },
                    data: [...nullList,...sellPrice],
                    symbol:'none',
                    type: 'line',
                    areaStyle: {}
                }]
            });
        },
        //组件
        components: {

        },
        //监视
        watch: {

        },
        //过滤器
        filters:{

        },
        //自定义指令
        directive:{

        }
    }
</script>

<!--
	作者：liangyanxiangde@163.com
	时间：2017-03-27
	描述：统一使用less,局部样式
-->
<style lang="less" scoped>
    .chart{
        width: 450px;
        height: 400px;
    }
</style>