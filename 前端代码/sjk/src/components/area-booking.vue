<template>
    <div>
        <div class="header">
            预约练车场地
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 100%" class="table" border>
                <el-table-column prop="area_id" label="场地ID" width="200" align="center">
                </el-table-column>
                <el-table-column prop="area_name" label="场地名称" width="200" align="center">
                </el-table-column>
                <el-table-column prop="capacity" label="最大容纳人数" width="200" align="center">
                </el-table-column>
                <el-table-column prop="avail_capacity" label="剩余容纳人数" width="175" align="center">
                </el-table-column>
                <el-table-column prop="operate" label="操作" width="208" align="center">
                    <template slot-scope="scope">
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia(scope.row)">预约场地</el-button>
                    </template>
                </el-table-column>
            </el-table>


            <el-dialog title="预约场地" :visible.sync="dialog" class="dialog" width="40%">
                <div>
                    <el-form ref="form" :model="form" label-width="100px">
                        <el-form-item label="场地ID：">
                            <span>{{ form.area_id }}</span>
                        </el-form-item>

                        <el-form-item label="场地名称：">
                            <span>{{ form.area_name }}</span>
                        </el-form-item>

                        <el-form-item label="预约人ID：">
                            <el-input v-model="form.areabooking_id"></el-input>
                        </el-form-item>

                        <el-form-item label="预约日期：">
                            <el-input v-model="form.areabooking_date"></el-input>
                        </el-form-item>
                        <el-form-item label="预约时间：">
                            <el-input v-model="form.areabooking_time"></el-input>
                        </el-form-item>
                        <el-form-item label="预约教练：">
                            <el-input v-model="form.instruct_name"></el-input>
                        </el-form-item>

                    </el-form>
                    <div style="text-align: center;">
                        <el-button type="primary" @click="add">
                            提交
                        </el-button>
                    </div>
                </div>
            </el-dialog>
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
            tableData: [],
            dialog: false,
            form: {
                area_id: '',
                area_name: '',
                capcity: '',
                avail_capcity:'',
            }
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/user/area",this.form).then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia(row) {
            this.form.area_id = row.area_id;
            this.form.area_name = row.area_name;
            this.form.capacity = row.capacity;
            this.form.avail_capacity = row.avail_capacity;
            this.dialog = true;
        },
        add() {
            this.$axios.post("/api/user/addareabookings", this.form).then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.$message({
                        message: "成功预约",
                        type: "success"
                    })
                    this.dialog = false;
                    this.getdata();
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
    border-bottom: 2px solid #e3e3e3;
}

.body {

    width: 68%;
    margin: auto;
    margin-top: 30px;
}

.el-table {
    width: 100%; /* 确保表格占满父容器 */
    table-layout: fixed; /* 防止溢出 */
}
</style>