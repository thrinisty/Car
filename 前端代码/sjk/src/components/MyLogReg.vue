<template>
    <div class="container">
        <div class="login_box">
            <div class="head">
                驾校考试训练预约平台
            </div>
            <!-- 登录 -->
            <div v-show="target == 1">
                <el-form label-width="0" class="login_form" :model="login_form" :rules="login_rules" ref="login_form">
                    <!-- 用户名 -->
                    <el-form-item prop="userortel">
                        <el-input v-model="login_form.userortel" spellcheck="false" placeholder="手机号">
                        </el-input>
                    </el-form-item>
                    <!-- 密码 -->
                    <el-form-item prop="password">
                        <el-input v-model="login_form.password" show-password spellcheck="false" placeholder="密码">
                        </el-input>
                    </el-form-item>


                    <!-- 按钮 -->
                    <el-form-item class="btns">
                        <el-button type="primary" @click="llogin()">登录</el-button>
                    </el-form-item>

                </el-form>
                <div>
                    <div class="operate">
                        <span id="op1" @click="change(2)">注册</span>
                    </div>
                </div>
            </div>
        </div>


        <!-- 注册表单 -->
        <div class="reg_box" v-show="target == 2">
            <div class="head">
                驾校考试训练预约平台
            </div>
            <div>
                <el-form class="reg_form" :model="reg_form" :rules="reg_rules" ref="reg_form">
                    <!-- 用户名 -->
                    <el-form-item prop="username">
                        <el-input prefix-icon="iconfont icon-user" v-model="reg_form.username" spellcheck="false"
                            placeholder="用户名">
                        </el-input>
                    </el-form-item>
                    <!-- 密码 -->
                    <el-form-item prop="password">
                        <el-input prefix-icon="iconfont icon-password" v-model="reg_form.password" show-password
                            spellcheck="false" placeholder="密码(包含大小写字母、数字，长度在6-12之间)"></el-input>
                    </el-form-item>

                    <el-form-item prop="telephone">
                        <el-input prefix-icon="iconfont icon-password" v-model="reg_form.telephone" spellcheck="false"
                            placeholder="手机号码"></el-input>
                    </el-form-item>

                    <!-- 按钮 -->
                    <el-form-item class="btns">
                        <el-button type="primary" @click="zhuce()">注册</el-button>
                    </el-form-item>

                </el-form>
                <div>
                    <div>
                        <span @click="change(1)"
                            style="margin-left:210px;color: #000;opacity: .5;font-weight: 400;font-size: 16px;cursor:pointer;">登录</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MyLogin',
    data() {
        var checkPassword = (rule, value, cb) => {
            const regPassword = /(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9]).{6,12}$/;
            if (regPassword.test(value)) {
                return cb()
            }
            cb(new Error('密码需包含大写字母、小写字母、数字，长度在6-12位之间'))
        };
        var checkMobile = (rule, value, cb) => {
            const regMobile = /^(0|86|17951)?(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$/;
            if (regMobile.test(value)) {
                return cb()
            }
            cb(new Error('手机号码格式不正确'))
        };
        return {
            target: 1,
            login_form: {
                userortel: '',
                password: '',
            },
            reg_form: {
                username: '',
                password: '',
                telephone: '',
            },
            login_rules: {
                userortel: [
                    { required: true, message: '请输入手机号', trigger: 'blur' }, { validator: checkMobile, trigger: 'blur' } ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' }]
            },
            reg_rules: {
                username: [{ required: true, message: '请设置用户名', trigger: 'blur' }],
                password: [{ required: true, message: '请设置密码', trigger: 'blur' }, { validator: checkPassword, trigger: 'blur' }],
                telephone: [{ required: true, message: '请绑定手机号', trigger: 'blur' }, { validator: checkMobile, trigger: 'blur' }]
            },
        }
    },
    methods: {
        zhuce() {
            this.$refs.reg_form.validate(valid => {
                if (!valid) return;
                else {
                    this.$axios.request({
                        method: 'POST',
                        url: '/api/user/register/test',
                        data: {
                            username: this.reg_form.username,
                            password: this.reg_form.password,
                            telephone: this.reg_form.telephone
                        }
                    }).then((res) => {
                        if (res.data.status == 200) {
                            this.$message({
                                message: '注册成功',
                                type: 'success'
                            })
                            this.target = 1;
                        } else {
                            this.$message({
                                message: res.data.msg,
                                type: 'error'
                            })
                        }
                    })
                }
            })
        },
        change(id) {
            this.target = id;
        },
        llogin() {
            this.$refs.login_form.validate(valid => {
                if (!valid) return;
                else //验证通过再发送请求
                    this.login();
            })
        },
        async login() {
            this.$axios.post("/api/user/login", this.login_form).then((res) => {
                if (res.data.code != 200) {
                    return this.$message({
                        message: res.data.msg,
                        type: 'error '
                    })
                } else {
                    this.$message({
                        message: '登录成功',
                        type: 'success'
                    })
                    window.localStorage.setItem("token", res.data.token);
                    if (res.data.role == 0)
                        this.$router.push('/user')
                    else
                        this.$router.push('/manage')
                }
            }).catch(() => {
                this.$message({
                    message: "网络故障",
                    type: 'error'
                })
            })
        },
    }
}
</script>

<style lang="less" scoped>
.container {
    background-color: #2b4b6b;
    height: 100%;
    width: 100%;
}

.head {
    text-align: center;
    height: 50px;
    line-height: 50px;
    font-size: larger;
}

.login_box {
    height: 300px;
    width: 450px;
    background-color: white;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.reg_box {
    height: 400px;
    width: 450px;
    background-color: white;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.input {
    width: 350px;
    height: 50px;
    margin-left: 50px;
}

.el-form-item {
    width: 350px;
    margin-left: 50px;
}

.btns {
    text-align: center;
}

.operate {
    text-align: center;
    color: #000;
    opacity: .5;
    font-weight: 400;
    font-size: 16px;
    margin-left: 28px;
}

#op1 {
    padding-left: 15px;
    padding-right: 30px;
    border-right: 1px solid #bdb9b9;
    cursor: pointer;
}

#op2 {
    padding-left: 30px;
    padding-right: 15px;
    cursor: pointer;
}
</style>
