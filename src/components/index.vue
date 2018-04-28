<template>
<div id="index">
    <div class="card_box">

        <el-card class="card" :body-style="{ padding: '0px' }" v-for="(i,k) in form_data" :key="(i,k)">
            <img :src="[i.attributes.form_data.img_arr[0]]" class="image">
            <div style="padding: 14px;">
                <span>{{i.attributes.form_data.title}}</span>
                <div class="bottom clearfix">
                    <time class="time">{{i.attributes.form_data.date.getMonth()+1}},{{i.attributes.form_data.date.getDate()}} {{i.attributes.form_data.date.getFullYear()}}</time>
                    <el-row class="row">
                        <el-col :span="12">
                            <el-button type="text" @click=" centerDialogVisible= true;del(i,k);" class="button left">删除</el-button>
                        </el-col>
                        <el-col :span="12">
                            <el-button type="text" class="button right">更多</el-button>
                        </el-col>
                    </el-row>
                </div>
            </div>
        </el-card>

    </div>

    <router-link to="/add">
        <el-button class="add" type="primary" icon="el-icon-plus" circle></el-button>
    </router-link>



</div>
</template>

<script>
import AV from "leancloud-storage";
import item from 'element-ui';
AV.init("FkqqYvsF0LriKkSIJb7E3ljX-gzGzoHsz", "ExgNcL3fCzi75uv5R4yJxVnM");
export default {
    name: "index",
    data() {
        return {
            form_data: [],
            centerDialogVisible: false,
            del_index: "",
            objectId: ""
        };
    },
    created() {
        let this_ = this;
        var query = new AV.Query('memo');
        query.descending('createdAt');
        query.find().then(todo => {
            this_.form_data = todo

            // console.log(todo.attributes)
        }).catch(error => {

        });
    },
    methods: {
        del(i,k) {
            let this_ = this;
            console.log(k)
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                var todo = AV.Object.createWithoutData('memo',i.id);
                todo.destroy().then(success => {
                    this.$message({
                        type: 'success',
                        message: '删除成功!'
                    });
                    this_.form_data.splice(k,1)
                }).catch(error => {
                });
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消删除'
                });
            });
        }
    }

};
</script>

<style scoped>
.card_box {
    margin-bottom: 30px
}

.time {
    font-size: 13px;
    color: #999;
}

.row {
    margin-top: 15px
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
    display: block;
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

.left {
    float: left;
}

.righ {
    float: right;
}

.add {
    width: 50px;
    height: 50px;
    position: fixed;
    right: 30px;
    bottom: 50px;
}
</style>
