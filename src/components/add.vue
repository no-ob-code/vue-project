<template>
<div id="add" v-loading.fullscreen.lock="loading" element-loading-text="图片上传中" element-loading-spinner="el-icon-loading" element-loading-background="rgba(0, 0, 0, 0.8)">
    <span v-if="num==100?loading=false:loading=true"></span>
    <el-form :label-position="labelPosition" :model="form_data">
        <el-form-item class="form-item" label="标题" label-width="50px">
            <el-input placeholder="请输入标题" v-model="form_data.title">
            </el-input>
        </el-form-item>
        <el-form-item class="form-item" label="内容" label-width="50px">
            <el-input placeholder="请输入内容" maxlength="120" autosize type="textarea" v-model="form_data.content">
            </el-input>
        </el-form-item>
        <el-form-item class="form-item" label="日期" label-width="50px">
            <el-date-picker v-model="form_data.date" type="date" placeholder="选择日期">
            </el-date-picker>
        </el-form-item>
        <el-form-item class="form-item" label="图片" label-width="50px">
        </el-form-item>
        <el-row :gutter="10">
            <el-col class="img_box" :span='6' v-for="(i,k) in form_data.img_arr" :key="i,k">
                <el-button class="del" @click="del(k)" circle>X</el-button>
                <img :src="i" alt="">
            </el-col>
            <el-col :span='6'>
                <el-button @click='click()' class="add" type="primary" icon="el-icon-plus"></el-button>
                <input type="file" style="display:none" id="files" @change="upload($event)">
            </el-col>
        </el-row>
    </el-form>
    <el-button type="primary" class="sure" @click="sure()">确认</el-button>
</div>
</template>

<script>
import AV from "leancloud-storage";

export default {
    name: "add",
    data() {
        return {
            loading: false,
            labelPosition: "right",
            form_data: {
                title: "",
                content: "",
                date: new Date(),
                img_arr: []
            },
            num: 100,
        };
    },
    methods: {

        click() {
            document.getElementById("files").click();
            if (this.form_data.img_arr.length >= 4) {
                alert("最多上传4张 !");
                return;
            }
        },
        upload(e) {
            let this_ = this;

            var reader = new FileReader();
            reader.onload = function (file) {};
            reader.readAsDataURL(e.target.files[0]);
            var fileUploadControl = e.target;
            if (fileUploadControl.files.length > 0) {
                var localFile = fileUploadControl.files[0];
                var name = "1.jpg";
                var file = new AV.File(name, localFile);
                file
                    .save({
                        onprogress: function (e) {
                            this_.num = e.percent
                        }
                    })
                    .then(
                        function (file) {
                            this_.form_data.img_arr.push(file.url());
                        },
                        function (error) {
                            // 异常处理
                            console.error(error);
                        }
                    );
            }
        },
        del(e) {
            this.form_data.img_arr.splice(e, 1)
        },
        sure() {
            let this_ = this
            console.log(this.form_data.title)
            if (this.form_data.title == "") {
                alert("标题不能为空 ！");
                return;
            }
            if (this.form_data.content == "") {
                alert("内容不能为空 ！");
                return;
            }
            if (this.form_data.img_arr.length == 0) {
                alert("图片不能为空 ！");
                return;
            }
            // 声明类型
            var TodoFolder = AV.Object.extend("memo");
            // 新建对象
            var todoFolder = new TodoFolder();
            // 设置名称
            todoFolder.set("form_data", this_.form_data);
            // 设置优先级
            todoFolder.set("priority", 1);
            todoFolder.save().then(todo => {
                this_.$message({
                    message: '发送成功',
                    type: 'success'
                });
                this_.$router.back(-1)

            }).catch(error => {

            })
        }
    }
};
</script>

<style scoped>
h1,
h2 {
    font-weight: normal;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}

p {
    margin: 0;
}

.form-item {
    text-align: center;
}

.add {
    width: 80px;
    height: 80px;
}

.file_label {
    width: 80px;
}

.img_box img {
    width: 100%;
    position: relative;
}

.del {
    width: 20px;
    height: 20px;
    font-size: 10px;
    padding: 0;
    background: red;
    color: #fff;
    float: right;
    position: relative;
    top: 10px;
    z-index: 100;
    border: 0
}

.sure {
    width: 70%;
    margin: 100px 0 0 15%;
}
</style>
