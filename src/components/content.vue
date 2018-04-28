<template>
<div id="memo_content">
    <div class="card_box">
        <el-card class="card" :body-style="{ padding: '0px' }">
            <el-carousel :autoplay="false" type="card" min-height="100px">
                <el-carousel-item v-for="i in form_data.img_arr" :key="i">
                    <img :src="[i]" alt="" class="image">
                </el-carousel-item>
            </el-carousel>
            <div style="padding: 14px;">
                <span>{{form_data.title}}</span>
                <div class="bottom clearfix">
                    <time class="time">{{form_data.date.getMonth()+1}}月 {{form_data.date.getDate()}}日 {{form_data.date.getFullYear()}}</time>
                   <p class="content">{{form_data.content}}</p>
                    <el-row class="row">
                        <el-col :span="24">
                            <el-button type="text" @click=" centerDialogVisible= true;del();" class="button right">删除</el-button>
                        </el-col>
                    </el-row>
                </div>
            </div>
        </el-card>
    </div>
</div>
</template>

<script>
import AV from "leancloud-storage";

export default {
    name: "memo_content",
    data() {
        return {
            form_data: {},
            query: ""
        };
    },
    created() {
        let this_ = this;
        var query = new AV.Query("memo");
        query.get(this.$route.query.id).then(req => {
            this_.form_data = req.attributes.form_data
            console.log(req)
        }).catch(error => {

        })
    },
    methods: {
        del() {
            let this_ = this;
            this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
                    confirmButtonText: "确定",
                    cancelButtonText: "取消",
                    type: "warning"
                })
                .then(() => {
                    var todo = AV.Object.createWithoutData('memo', this_.$route.query.id);
                    todo.destroy().then(success => {
                        this.$message({
                            type: 'success',
                            message: '删除成功!'
                        });
                        this_.$route.back(-1)
                    }).catch(error => {});
                })
                .catch(() => {
                    this.$message({
                        type: "info",
                        message: "已取消删除"
                    });
                });
        },
    }
};
</script>

<style>
.card_box {
    margin-bottom: 30px;
}

.time {
    font-size: 13px;
    color: #999;
}

.row {
    margin-top: 15px;
}

.bottom {
    margin-top: 13px;
    line-height: 12px;
}

.button {
    padding: 0;
    float: right;
}

.image {
    width: 100%;
    display: inline-block;
    /* text-align: center; */
}

.clearfix:before,
.clearfix:after {
    display: table;
    content: "";
}

.clearfix:after {
    clear: both;
}

.card {
    margin-top: 20px;
}

.content{
    margin-top: 20px
}
.righ {
    float: right;
}

.el-carousel__item{
  line-height: 400px;
background-color: #99a9bf;
   /* top: 25% */
}

.el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
}

/* .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
} */
/* .el-carousel__container */
.el-carousel__container{
   background-color: #99a9bf;
}
/* .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
} */
</style>
