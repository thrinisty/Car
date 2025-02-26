<template>
    <div>
        <div class="header">
            个人信息
        </div>
        <div class="body">
            <el-form ref="form" :model="form" label-width="20%" id="selectForm">
                <el-form-item label="用户名：" prop="user_name">
                    <el-input v-model="form.user_name" ></el-input>
                </el-form-item>
                <el-form-item label="真实姓名：" prop="real_name">
                    <el-input v-model="form.real_name" placeholder="请输入真实姓名"></el-input>
                </el-form-item>
                <el-form-item label="年龄：" prop="age">
                    <el-input v-model="form.age" placeholder="请输入年龄"></el-input>
                </el-form-item>
                <el-form-item label="性别：" prop="sex">
                    <el-select v-model="form.sex" placeholder="请选择性别">
                        <el-option label="男" value="男"></el-option>
                        <el-option label="女" value="女"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="电话：" prop="phone">
                    <el-input v-model="form.phone" ></el-input>
                </el-form-item>
                <el-form-item label="邮箱：" prop="mail">
                    <el-input v-model="form.mail" placeholder="请输入邮箱"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="saveData">保存信息</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    created() {
        this.getdata();
    },
    data() {
        return {
            form: {
                real_name: '',
                sex: '',
                age: '',
                mail: '',
                phone: '',
                user_name: '',
            },
        };
    },
    methods: {
        getdata() {
            this.$axios.get("/api/user/usermsg").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.form.age = res.data.data.age || '';
                    this.form.mail = res.data.data.mail || '';
                    this.form.phone = res.data.data.phone || '';
                    this.form.real_name = res.data.data.real_name || '';
                    this.form.sex = res.data.data.sex || '';
                    this.form.user_name = res.data.data.user_name || '';
                }
            });
        },
        saveData() {
            this.$axios.post("/api/user/usermsg/update", this.form).then((res) => {
                if (res.data.status == 200) {
                    this.$message.success("信息保存成功！");
                } else {
                    this.$message.error("信息保存失败：" + res.data.msg);
                }
            });
        },
    },
};
</script>

<style scoped>
.header {
    width: 100%;
    height: 10%;
    text-align: center;
    line-height: 64px;
    font-size: 20px;
    font-weight: 800;
    border-bottom: 1px solid #e3e3e3;
}

.body {
    width: 40%;
    margin-top: 30px;
    margin-left: 30px;
}

#selectForm >>> .el-form-item__label {
    font-size: 18px;
}

span {
    font-size: 18px;
}
</style>
