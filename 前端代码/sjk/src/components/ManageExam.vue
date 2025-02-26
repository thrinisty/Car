<template>
    <div>
        <div class="header">
            考试信息
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 89%" class="table">
                <el-table-column prop="exam_id" label="考试编号" width="150" align="center">
                </el-table-column>
                <el-table-column prop="exam_name" label="考试名称" width="100" align="center">
                </el-table-column>
                <el-table-column prop="exam_date" label="考试日期" width="100" align="center">
                </el-table-column>
                <el-table-column prop="exam_time" label="考试时间" width="200" align="center">
                </el-table-column>
                <el-table-column prop="location" label="考试地点" width="200" align="center">
                </el-table-column>
                <el-table-column prop="total_slots" label="总名额" width="200" align="center">
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
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia_add()">添加考试
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>

            <el-dialog title="添加考试" :visible.sync="dia_add" width="30%">
                <el-form ref="add_form" :model="add_form" label-width="100px" :rules="add_form_rules">
                    <el-form-item label="考试编号：" prop="exam_id">
                        <el-input v-model="add_form.exam_id"></el-input>
                    </el-form-item>
                    <el-form-item label="考试名称" prop="exam_name">
                        <el-input v-model="add_form.exam_name"></el-input>
                    </el-form-item>
                    <el-form-item label="考试日期：" prop="exam_date">
                        <el-input v-model="add_form.exam_date"></el-input>
                    </el-form-item>
                    <el-form-item label="考试时间：" prop="exam_time">
                        <el-input v-model="add_form.exam_time"></el-input>
                    </el-form-item>
                    <el-form-item label="考试地点：" prop="location">
                        <el-input v-model="add_form.location"></el-input>
                    </el-form-item>
                    <el-form-item label="总名额：" prop="total_slots">
                        <el-input v-model="add_form.total_slots"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="add()">
                        添加
                    </el-button>
                </div>
            </el-dialog>

            <el-dialog title="修改考试信息" :visible.sync="dia_chg" width="30%">
                <el-form ref="form" :model="chg_form" label-width="100px">
                    <el-form-item label="考试编号：" prop="exam_id">
                        <el-input v-model="chg_form.exam_id"></el-input>
                    </el-form-item>
                    <el-form-item label="考试名称" prop="exam_name">
                        <el-input v-model="chg_form.exam_name"></el-input>
                    </el-form-item>
                    <el-form-item label="考试日期：" prop="exam_date">
                        <el-input v-model="chg_form.exam_date"></el-input>
                    </el-form-item>
                    <el-form-item label="考试时间：" prop="exam_time">
                        <el-input v-model="chg_form.exam_time"></el-input>
                    </el-form-item>
                    <el-form-item label="考试地点：" prop="location">
                        <el-input v-model="chg_form.location"></el-input>
                    </el-form-item>
                    <el-form-item label="总名额：" prop="total_slots">
                        <el-input v-model="chg_form.total_slots"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="change()">
                        修改
                    </el-button>
                </div>
            </el-dialog>
            <el-dialog title="删除考试信息" :visible.sync="dia_dlt" width="30%">
                <div>
                    确定删除这场考试的信息吗？
                </div>
                <div style="text-align: center;">
                    <el-button type="primary" @click="deleteexam()">
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
                exam_id: '',
                exam_name: '',
                exam_date: '',
                exam_time: '',
                location: '',
                total_slots:'',
                action:"add",
            },
            chg_form: {
                exam_id: '',
                exam_name: '',
                exam_date: '',
                exam_time: '',
                location: '',
                total_slots:'',
                action:"change",
            },
            want_delete: '',
            add_form_rules: {
                exam_id: [{ required: true, message: '必填项', trigger: 'blur' }],
                exam_name: [{ required: true, message: '必填项', trigger: 'blur' }],
                exam_date:[{ required: true, message: '必填项', trigger: 'blur' }],
                exam_time: [{ required: true, message: '必填项', trigger: 'blur' }],
                location: [{ required: true, message: '必填项', trigger: 'blur' }],
                total_slots:[{ required: true, message: '必填项', trigger: 'blur' }]

            }
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/manager/exam").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia_add() {
            this.dia_add = true;
        },
        add() {
            this.$refs.add_form.validate(valid => {
                if (!valid)
                    return;
                else //验证通过再发送请求
                    this.$axios.post("/api/manager/exam", this.add_form).then((res) => {
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
            this.chg_form.exam_id = row.exam_id;
            this.chg_form.exam_name = row.exam_name;
            this.chg_form.exam_date = row.exam_date;
            this.chg_form.exam_time = row.exam_time;
            this.chg_form.location = row.location;
            this.chg_form.total_slots = row.total_slots;
            this.dia_chg = true;
        },
        change() {
            this.$axios.post("/api/manager/exam", this.chg_form).then((res) => {
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
            this.want_delete = row.exam_id;
            this.dia_dlt = true;
        },
        deleteexam() {
            this.$axios.delete("/api/manager/exam", { data: { want_delete: this.want_delete } }).then((res) => {
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