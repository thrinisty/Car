<template>
    <div>
        <div class="header">
            &nbsp;&nbsp; 驾校考试训练预约
        </div>
        <div class="body">
            <!-- 左侧导航栏 -->
            <div class="liner">
                <el-menu default-active="1" class="el-menu-vertical-demo" background-color="#545c64" text-color="#fff"
                    active-text-color="#ffd04b" @select="handleselect">
                    <el-menu-item index="1">
                        <i class="el-icon-menu"></i>
                        <span slot="title">预约考试</span>
                    </el-menu-item>

                    <el-menu-item index="2">
                            <i class="el-icon-menu"></i>
                            <span>预约练车场地</span>

                            
                    </el-menu-item>

                    <el-submenu>
                        <template slot="title">
                            <i class="el-icon-s-home"></i>
                            <span>个人中心</span>
                        </template>
                        <el-menu-item-group>

                            <el-menu-item index="3">个人信息</el-menu-item>
                            <el-menu-item index="4">修改密码</el-menu-item>
                        </el-menu-item-group>

                    </el-submenu>

                </el-menu>
            </div>
            <div class="main">
                <div id="exambooking" v-show="active == 1">
                    <exambooking></exambooking>
                </div>

                <div id="userfinished" v-show="active == 2">
                    <areabooking></areabooking>
                </div>

                <div id="indimag" v-show="active == 3">
                    <indimsg></indimsg>
                </div>

                <div id="changepwd" v-show="active == 4">
                    <changepwd></changepwd>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
import exambooking from '@/components/exam-booking.vue'
import areabooking from '@/components/area-booking.vue'
import indimsg from '@/components/UserMsg/IndiMsg.vue'
import changepwd from '@/components/UserMsg/ChPwd.vue'
export default {
    components: {
        exambooking: exambooking,
        areabooking: areabooking,
        indimsg: indimsg,
        changepwd: changepwd,
    },
    data() {
        return {
            active: 1,
        };
    },
    methods: {
        handleselect(index) {
            this.active = index;
        }
    },
}  
</script>

<style scoped>
.header {
    width: 100%;
    height: 10vh;
    /* text-align: center; */
    line-height: 10vh;
    font-size: 25px;
    font-weight: 800;
    background-color: #dababa;
    /* padding-left: 100px; */
}

.body {
    width: 100%;
    height: 648px;
    display: flex;
    justify-content: space-around;
}

.liner {
    width: 15%;
    height: 100%;
    background-color: #298888;
}

.main {
    width: 85%;
}
</style>