<template>
    <div>
        <div class="header">
            &nbsp;&nbsp; 信息管理
        </div>
        <div class="body">
            <div class="liner">
                <el-menu default-active="1" class="el-menu-vertical-demo" background-color="#545c64" text-color="#fff"
                    active-text-color="#ffd04b" @select="handleselect">

                    <el-menu-item index="1">
                        <i class="el-icon-s-custom"></i>
                        <span slot="title">学员管理</span>
                    </el-menu-item>

                    <el-menu-item index="2">
                        <i class="el-icon-s-custom"></i>
                        <span slot="title">教练员管理</span>
                    </el-menu-item>

                    <el-menu-item index="3">
                        <i class="el-icon-s-shop"></i>
                        <span slot="title">场地管理</span>
                    </el-menu-item>

                    <el-menu-item index="4">
                        <i class="el-icon-user"></i>
                        <span slot="title">考试信息</span>
                    </el-menu-item>

                </el-menu>
            </div>
            <div class="main">
                <div id="managestu" v-show="active == 1">
                    <managestu></managestu>
                </div>

                <div id="manageintr" v-show="active == 2">
                    <manageinstr></manageinstr>
                </div>

                <div id="managearea" v-show="active == 3">
                    <managearea></managearea>
                </div>


                <div id="managexam" v-show="active == 4">
                    <managexam></managexam>
                </div>

                </div>
            </div>
        </div>

</template>

<script>
import managestu from '@/components/ManageStu.vue'
import manageinstr from '@/components/ManageInstr.vue'
import managearea from '@/components/ManageArea.vue'
import managexam from '@/components/ManageExam.vue'

export default {
    components: {
        managestu: managestu,
        manageinstr: manageinstr,
        managearea: managearea,
        managexam: managexam,
    },
    data() {
        return {
            active: 1,
        }
    },
    methods: {
        handleselect(index) {
            this.active = index;
        }
    },
}
</script>

<style scoped>
/* 优化后的样式 */
.header {
    width: 100%;
    height: 10vh;
    line-height: 10vh;
    font-size: 25px;
    font-weight: 800;
    background: linear-gradient(to right, #ac8b8b, #d19f8b);
    color: white;
    text-align: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.body {
    width: 100%;
    height: 648px;
    display: flex;
    justify-content: space-around;
    background: url('https://via.placeholder.com/1920x1080') no-repeat center center;
    background-size: cover;
    color: white;
}

.liner {
    width: 15%;
    height: 100%;
    background-color: #c4bcbc;
    border-right: 1px solid #ddd;
    box-shadow: 2px 0 5px rgba(173, 172, 175, 0.1);
    border-radius: 8px 0 0 8px;
    padding-top: 20px;
}

.liner .el-menu-item {
    transition: background-color 0.3s, transform 0.2s;
}

.liner .el-menu-item:hover {
    background-color: #134d83;
    transform: scale(1.05);
}

.main {
    width: 85%;
    padding: 20px;
    background-color: white;
    border-radius: 0 8px 8px 0;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.main > div {
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
}
</style>
