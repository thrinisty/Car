<template>
    <div>
        <div class="header">
            场地管理
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 89%" class="table">
                <el-table-column prop="area_id" label="场地ID" width="200" align="center">
                </el-table-column>
                <el-table-column prop="area_name" label="场地名称" width="200" align="center">
                </el-table-column>
                <el-table-column prop="capacity" label="最大容纳人数" width="200" align="center">
                </el-table-column>
                <el-table-column prop="operate" label="操作" width="200" align="center">
                    <template slot-scope="scope">
                        <el-button size="small" type="warning" @click="showdia_chg(scope.row)">修改
                        </el-button>

                        <el-button size="small" type="danger" @click="showdia_dlt(scope.row)">删除
                        </el-button>
                    </template>
                </el-table-column>
                <el-table-column width="120" align="center">
                    <template slot="header">
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia_add()">添加场地
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>

            <el-dialog title="添加场地" :visible.sync="dia_add" width="30%">
                <el-form ref="add_form" :model="add_form" label-width="120px" :rules="add_form_rules">
                    <el-form-item label="场地ID：" prop="area_id">
                        <el-input v-model="add_form.area_id"></el-input>
                    </el-form-item>
                    <el-form-item label="场地名称：" prop="area_name">
                        <el-input v-model="add_form.area_name"></el-input>
                    </el-form-item>
                    <el-form-item label="最大容纳人数：" prop="capacity">
                        <el-input v-model="add_form.capacity"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="addarea()">
                        添加
                    </el-button>
                </div>
            </el-dialog>

            <el-dialog title="修改场地信息" :visible.sync="dia_chg" width="30%">
                <el-form ref="form" :model="chg_form" label-width="100px">
                    <el-form-item label="场地ID：" prop="area_id">
                        <el-input v-model="chg_form.area_id"></el-input>
                    </el-form-item>
                    <el-form-item label="场地名称：" prop="area_name">
                        <el-input v-model="chg_form.area_name"></el-input>
                    </el-form-item>
                    <el-form-item label="最大容纳人数：" prop="capacity">
                        <el-input v-model="chg_form.capacity"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="changearea()">
                        修改
                    </el-button>
                </div>
            </el-dialog>
            <el-dialog title="删除场地" :visible.sync="dia_dlt" width="30%">
                <div>
                    确定删除此场地吗？
                </div>
                <div style="text-align: center;">
                    <el-button type="primary" @click="deletearea()">
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
                area_id: '',
                area_name: '',
                capacity: '',
                action: "add",
            },
            chg_form: {
                area_id: '',
                area_name: '',
                capacity: '',
                action: "change",
            },
            want_delete: '',
            add_form_rules: {
                area_id: [{ required: true, message: '必填项', trigger: 'blur' }],
                area_name: [{ required: true, message: '必填项', trigger: 'blur' }],
                capacity: [{ required: true, message: '必填项', trigger: 'blur' }]

            },

        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/manager/area").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia_add() {
            this.dia_add = true;
        },
        addarea() {
            this.$refs.add_form.validate(valid => {
                if (!valid)
                    return;
                else //验证通过再发送请求
                    this.$axios.post("/api/manager/area", this.add_form).then((res) => {
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
            this.chg_form.area_id = row.area_id;
            this.chg_form.area_name = row.area_name;
            this.chg_form.capacity = row.capacity;
            this.dia_chg = true;
        },
        changearea() {
            this.$axios.post("/api/manager/area", this.chg_form).then((res) => {
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
            this.want_delete = row.area_id;
            this.dia_dlt = true;
        },
        deletearea() {
            this.$axios.delete("/api/manager/area", { data: { want_delete: this.want_delete } }).then((res) => {
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