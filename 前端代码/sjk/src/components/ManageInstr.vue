<template>
    <div>
        <div class="header">
            教练员管理
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 89%" class="table">
                <el-table-column prop="instruct_name" label="教练姓名" width="200" align="center">
                </el-table-column>
                <el-table-column prop="instruct_id" label="教练ID" width="200" align="center">
                </el-table-column>
                <el-table-column prop="phone" label="教练电话" width="200" align="center">
                </el-table-column>
                <el-table-column prop="hire_date" label="雇佣日期" width="200" align="center">
                </el-table-column>
                <el-table-column prop="operate" label="操作" width="200" align="center">
                    <template slot-scope="scope">
                        <el-button size="small" type="warning" @click="showdia_chg(scope.row)">修改
                        </el-button>

                        <el-button size="small" type="danger" @click="showdia_dlt(scope.row)">解雇
                        </el-button>
                    </template>
                </el-table-column>
                <el-table-column width="120" align="center">
                    <template slot="header">
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia_add()">添加教练员
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>

            <el-dialog title="添加教练员" :visible.sync="dia_add" width="30%">
                <el-form ref="add_form" :model="add_form" label-width="120px" :rules="add_form_rules">
                    <el-form-item label="教练姓名：" prop="instuct_name">
                        <el-input v-model="add_form.instruct_name"></el-input>
                    </el-form-item>
                    <el-form-item label="教练ID：" prop="instruct_id">
                        <el-input v-model="add_form.instruct_id"></el-input>
                    </el-form-item>
                    <el-form-item label="电话：" prop="phone">
                        <el-input v-model="add_form.phone"></el-input>
                    </el-form-item>
                    <el-form-item label="雇佣日期：" prop="hire_date">
                        <el-input v-model="add_form.hire_date"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="addinstr()">
                        添加
                    </el-button>
                </div>
            </el-dialog>
            <el-dialog title="修改教练信息" :visible.sync="dia_chg" width="30%">
                <el-form ref="form" :model="chg_form" label-width="100px">
                    <el-form-item label="教练姓名：" prop="instuct_name">
                        <el-input v-model="chg_form.instruct_name"></el-input>
                    </el-form-item>
                    <el-form-item label="教练ID：" prop="instruct_id">
                        <el-input v-model="chg_form.instruct_id"></el-input>
                    </el-form-item>
                    <el-form-item label="电话：" prop="phone">
                        <el-input v-model="chg_form.phone"></el-input>
                    </el-form-item>
                    <el-form-item label="雇佣日期：" prop="hire_date">
                        <el-input v-model="chg_form.hire_date"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="changeinstr()">
                        修改
                    </el-button>
                </div>
            </el-dialog>

            <el-dialog title="解雇教练员" :visible.sync="dia_dlt" width="30%">
                <div>
                    确定解雇此教练员吗？
                </div>
                <div style="text-align: center;">
                    <el-button type="primary" @click="deleteinstr()">
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
            dia_dlt: false,
            dia_chg: false,
            add_form: {
                instruct_id: '',
                instruct_name: '',
                phone: '',
                hire_date:'',
                action:"add",
            },
            chg_form: {
                instruct_id: '',
                instruct_name: '',
                phone: '',
                hire_date:'',
                action:"change",
            },
            want_delete: '',
            add_form_rules: {
                instruct_id: [{ required: true, message: '必填项', trigger: 'blur' }],
                instruct_name: [{ required: true, message: '必填项', trigger: 'blur' }],
                phone: [{ required: true, message: '必填项', trigger: 'blur' }],
                hire_date :[{ required: true, message: '必填项', trigger: 'blur' }]  

            },
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/manager/instructors").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia_add() {
            this.dia_add = true;
        },
        addinstr() {
            this.$refs.add_form.validate(valid => {
                if (!valid)
                    return;
                else //验证通过再发送请求
                    this.$axios.post("/api/manager/instructors", this.add_form).then((res) => {
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
            this.chg_form.instruct_id = row.instruct_id;
            this.chg_form.instruct_name = row.instruct_name;
            this.chg_form.phone = row.phone;
            this.chg_form.hire_date = row.hire_date;
            this.dia_chg = true;
        },
        changeinstr() {
            this.$axios.post("/api/manager/instructors", this.chg_form).then((res) => {
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
            this.want_delete = row.instruct_id;
            this.dia_dlt = true;
        },
        deleteinstr() {
            this.$axios.delete("/api/manager/instructors", { data: { want_delete: this.want_delete } }).then((res) => {
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