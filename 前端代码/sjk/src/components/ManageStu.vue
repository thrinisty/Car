<template>
    <div>
        <div class="header">
            学员管理
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 89%" class="table">
                <el-table-column prop="student_name" label="学员姓名" width="150" align="center">
                </el-table-column>
                <el-table-column prop="student_id" label="学员ID" width="100" align="center">
                </el-table-column>
                <el-table-column prop="gender" label="性别" width="100" align="center">
                </el-table-column>
                <el-table-column prop="phone" label="电话" width="200" align="center">
                </el-table-column>
                <el-table-column prop="registration_date" label="报名日期" width="200" align="center">
                </el-table-column>
                <el-table-column prop="operate" label="操作" width="150" align="center">
                    <template slot-scope="scope">
                        <el-button size="small" type="warning" @click="showdia_chg(scope.row)">修改
                        </el-button>

                        <el-button size="small" type="danger" @click="showdia_dlt(scope.row)">删除
                        </el-button>
                    </template>
                </el-table-column>
                <el-table-column width="120" align="center">
                    <template slot="header">
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia_add()">添加学员
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>

            <el-dialog title="添加学员" :visible.sync="dia_add" width="30%">
                <el-form ref="add_form" :model="add_form" label-width="100px" :rules="add_form_rules">
                    <el-form-item label="学员姓名：" prop="student_name">
                        <el-input v-model="add_form.student_name"></el-input>
                    </el-form-item>
                    <el-form-item label="学员ID：" prop="student_id">
                        <el-input v-model="add_form.student_id"></el-input>
                    </el-form-item>
                    <el-form-item label="性别：" prop="gender">
                        <el-input v-model="add_form.gender"></el-input>
                    </el-form-item>
                    <el-form-item label="电话：" prop="phone">
                        <el-input v-model="add_form.phone"></el-input>
                    </el-form-item>
                    <el-form-item label="报名日期：" prop="registration_date">
                        <el-input v-model="add_form.registration_date"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="addshop()">
                        添加
                    </el-button>
                </div>
            </el-dialog>

            <el-dialog title="修改学员" :visible.sync="dia_chg" width="30%">
                <el-form ref="form" :model="chg_form" label-width="100px">
                    <el-form-item label="学员姓名：">
                        <el-input v-model="chg_form.student_name"></el-input>
                    </el-form-item>
                    <el-form-item label="学员ID：">
                        <el-input v-model="chg_form.student_id"></el-input>
                    </el-form-item>
                    <el-form-item label="性别：">
                        <el-input v-model="chg_form.gender"></el-input>
                    </el-form-item>
                    <el-form-item label="电话：">
                        <el-input v-model="chg_form.phone"></el-input>
                    </el-form-item>
                    <el-form-item label="报名日期：">
                        <el-input v-model="chg_form.registration_date"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="changeshop()">
                        修改
                    </el-button>
                </div>
            </el-dialog>
            <el-dialog title="删除学员" :visible.sync="dia_dlt" width="30%">
                <div>
                    确定删除此学员信息吗？
                </div>
                <div style="text-align: center;">
                    <el-button type="primary" @click="deleteshop()">
                        确定
                    </el-button>
                </div>
            </el-dialog>
        </div>
    </div>
</template>

<script>
export default {
    created() {
        this.getdata()
    },
    data() {
        return {
            tableData: [],
            dia_add: false,
            dia_chg: false,
            dia_dlt: false,
            add_form: {
                student_name: '',
                student_id: '',
                gender:'',
                phone: '',
                registration_date:'',
                action: "add",
            },
            chg_form: {
                student_name: '',
                student_id: '',
                gender:'',
                phone: '',
                registration_date:'',
                action: "change",
            },
            want_delete: '',
            add_form_rules: {
                student_name: [{ required: true, message: '必填项', trigger: 'blur' }],
                student_id: [{ required: true, message: '必填项', trigger: 'blur' }],
                gender:[{ required: true, message: '必填项', trigger: 'blur' }],
                phone: [{ required: true, message: '必填项', trigger: 'blur' }],
                registration_date: [{ required: true, message: '必填项', trigger: 'blur' }],

            }
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/manager/student").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia_add() {
            this.dia_add = true;
        },
        addshop() {
            this.$refs.add_form.validate(valid => {
                if (!valid)
                    return;
                else //验证通过再发送请求
                    this.$axios.post("/api/manager/student", this.add_form).then((res) => {
                        console.log(res.data);
                        if (res.data.status == 200) {
                            this.$message({
                                message: "添加成功",
                                type: "success"
                            })
                            this.dia_add = false;
                            this.getdata();
                        } else {
                            this.$message({
                                message: res.data.msg,
                                type: "error"
                            })
                        }
                    })
            })


        },
        showdia_chg(row) {
            this.chg_form.student_name = row.student_name;
            this.chg_form.student_id = row.student_id;
            this.chg_form.gender = row.gender;
            this.chg_form.phone = row.phone;
            this.chg_form.registration_date = row.registration_date;
            this.dia_chg = true;
        },
        changeshop() {
            this.$axios.post("/api/manager/student", this.chg_form).then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.$message({
                        message: "修改成功",
                        type: "success"
                    })
                    this.dia_chg = false;
                    this.getdata();
                }
            })
        },
        showdia_dlt(row) {
            this.want_delete = row.student_id;
            this.dia_dlt = true;
        },
        deleteshop() {
            this.$axios.delete("/api/manager/student", { data: { want_delete: this.want_delete } }).then((res) => {
                if (res.data.status == 200) {
                    this.$message({
                        message: res.data.msg,
                        type: "success"
                    })
                    this.getdata()
                    this.dia_dlt = false;
                }
            })
        }
    }
}
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

    width: 80%;
    margin: auto;
    margin-top: 30px;
}
</style>