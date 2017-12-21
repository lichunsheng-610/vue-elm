<template>
    <div>
        <div class="demo_fade">
            <el-button @click="show = !show">Click Me</el-button>
            <div style="display: flex; margin-top: 20px; height: 100px;">
                <transition name="el-fade-in-linear">
                    <div v-show="show" class="transition-box">.el-fade-in-linear</div>
                </transition>
                <transition name="el-fade-in">
                    <div v-show="show" class="transition-box">.el-fade-in</div>
                </transition>
            </div>
        </div>
        <div class="demo_table">
            <el-button @click="searchBtn">查询</el-button>
            <el-table :data="tableData3" height="350" border style="width: 100%">
                <el-table-column prop="date" label="日期" width="180"> </el-table-column>
                <el-table-column prop="name" label="姓名" width="180"> </el-table-column>
                <el-table-column prop="address" label="地址"> </el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            show: true,
            tableData3: []
        }
    },
    created() {
        Mock.mock('test', {
            "name": '@name', //模拟名称
            "age|1-100": 100, //模拟年龄(1-100)
            "color": "@color", //模拟色值
            "date": "@date('yyyy-MM-dd')", //模拟时间
            "url": "@url()", //模拟url
            "content": "@cparagraph()" //模拟文本
        });

        Mock.mock("table_list", {
            "data|1-10": [
                {
                    "name": '@name', //模拟名称
                    "date": "@date('yyyy-MM-dd')", //模拟时间
                    "address": "@county(true)" //模拟文本
                }
            ]
        })
    },
    methods: {
        searchBtn() {
            var _this = this;
            $.ajax({
                url: "table_list", //请求的url地址
                dataType: "json", //返回格式为json
                async: true, //请求是否异步，默认为异步，这也是ajax重要特性
                data: {}, //参数值
                type: "GET", //请求方式
                beforeSend: function () {
                    //请求前的处理
                    // console.log('start');
                },
                success: function (req) {
                    //请求成功时处理
                    console.log(req.data);
                    _this.tableData3 = req.data;
                },
                complete: function () {
                    //请求完成的处理
                    // console.log('end');
                },
                error: function (err) {
                    //请求出错处理
                    // console.log(err);
                }
            });
        }
    }
}
</script>

<style>
.transition-box {
  font-size: 18px;
  margin-bottom: 10px;
  width: 200px;
  height: 100px;
  border-radius: 4px;
  background-color: #409eff;
  text-align: center;
  color: #fff;
  padding: 40px 20px;
  box-sizing: border-box;
  margin-right: 20px;
}
.demo_table {
  width: 1000px;
}
</style>