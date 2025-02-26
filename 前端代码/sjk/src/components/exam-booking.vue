<template>
    <div>
        <div class="header">
            欢迎预约
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 200%" class="table" border>
                <el-table-column prop="exam_id" label="考试编号" width="200" align="center"></el-table-column>
                <el-table-column prop="exam_name" label="考试名称" width="200" align="center"></el-table-column>
                <el-table-column prop="exam_date" label="考试日期" width="200" align="center"></el-table-column>
                <el-table-column prop="exam_time" label="考试时间" width="200" align="center"></el-table-column>
                <el-table-column prop="location" label="考试地点" width="200" align="center"></el-table-column>
                <el-table-column prop="total_slots" label="总名额" width="100" align="center"></el-table-column>
                <el-table-column prop="available_slots" label="剩余名额" width="100" align="center"></el-table-column>
                <el-table-column prop="operate" label="操作" width="208" align="center">
                    <template slot-scope="scope">
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia(scope.row)">预约考试</el-button>
                    </template>
                </el-table-column>
            </el-table>


            <el-dialog title="预约考试" :visible.sync="dialog" class="dialog" width="40%">
                <div>
                    <el-form ref="form" :model="form" label-width="100px">
                        <el-form-item label="考试ID：">
                            <span>{{ form.exam_id }}</span>
                        </el-form-item>

                        <el-form-item label="预约人姓名：">
                            <el-input v-model="form.booking_name"></el-input>
                        </el-form-item>

                        <el-form-item label="预约人ID：">
                            <el-input v-model="form.booking_id"></el-input>
                        </el-form-item>

                        <el-form-item label="考试日期：">
                            <span>{{ form.exam_date }}</span>
                        </el-form-item>
                        <el-form-item label="考试时间：">
                            <span>{{ form.exam_time }}</span>
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
                exam_id: '',
                exam_name: '',
                exam_date: '',
                // cons_phone: '',
                exam_time: '',
                location: '',
                total_slots:'',
                available_slots:'',
            }
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/user/exams",this.form).then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia(row) {
            this.form.exam_id = row.exam_id;
            this.form.exam_name = row.exam_name;
            this.form.exam_date = row.exam_date;
            this.form.exam_time = row.exam_time;
            this.form.location = row.location;
            this.form.total_slots = row.total_slots;
            this.form.available_slots = row.available_slots;
            this.dialog = true;
        },
        add() {
            this.$axios.post("/api/user/addbookings", this.form).then((res) => {
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
    border-bottom: 1px solid #e3e3e3;
}

.body {
    width: 90%;
    margin: auto;
    margin-top: 30px;
}

/* .table {
    margin-left: 120px;
} */

.dialog {
    /* width: 700px; */
}
</style>