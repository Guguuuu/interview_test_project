<template>
    <div class="UserContainer" v-if="visible">
        <div class="mask"></div>
        <div class="createUserBox">
            <h3>新建/编辑用户</h3>
            <div class="CreateUser-body w">
                <div class="item">
                    <span>姓名</span><br>
                    <input type="text" v-model="username">
                </div>
                <div class="item">
                    <span>年龄</span><br>
                    <input type="number" v-model="userage">
                </div>
                <div class="item">
                    <span>性别</span><br>
                    <select v-model="usersex">
                        <option value="男">男</option>
                        <option value="女">女</option>
                    </select>
                </div>
                <div class="item">
                    <span>联系电话</span><br>
                    <input type="tel" v-model="phoneNumber">
                </div>
                <div class="item">
                    <span>详细地址</span>
                    <div class="flexbox">
                        <el-select v-model="shen" multiple placeholder="广东省">
                            <el-option v-for="item in options1" :key="item.value" :label="item.label"
                                :value="item.label">
                            </el-option>
                        </el-select>
                        <el-select v-model="shi" multiple placeholder="广州市">
                            <el-option v-for="item in options2" :key="item.value" :label="item.label"
                                :value="item.label">
                            </el-option>
                        </el-select>
                        <el-select v-model="qu" multiple placeholder="番禺区">
                            <el-option v-for="item in options3" :key="item.value" :label="item.label"
                                :value="item.label">
                            </el-option>
                        </el-select>
                    </div>
                </div>
                <div class="item">
                    <br>
                    <el-input v-model="detail" placeholder="请输入详细地址"></el-input>
                </div>
            </div>

            <div class="CreateUser-footer w">
                <button type="button" @click="onConfirm">保存</button>
                <button type="button" @click="onClose">取消</button>
            </div>
        </div>
    </div>
</template>

<script>
import { nanoid } from 'nanoid'
export default {
    name: 'CreateUser',
    props: {
        visible: {
            type: Boolean,
            default: false
        },
        OneData: {
            type: Array,
            default: () => []
        }
    },
    data() {
        return {
            //根据需求收集用户的各种信息
            username: '',
            userage: '',
            usersex: '',
            phoneNumber: '',
            shen: '',
            shi: '',
            qu: '',
            detail: '',
            options1: [
                {
                    value: '选项1',
                    label: '广东省'
                },
                {
                    value: '选项2',
                    label: '其他省'
                }
            ],
            options2: [
                {
                    value: '选项1',
                    label: '广州市'
                },
                {
                    value: '选项2',
                    label: '其他市'
                }
            ],
            options3: [
                {
                    value: '选项1',
                    label: '番禺区'
                },
                {
                    value: '选项2',
                    label: '越秀区'
                },
                {
                    value: '选项3',
                    label: '花都区'
                },
                {
                    value: '选项4',
                    label: '海珠区'
                },
                {
                    value: '选项5',
                    label: '天河区'
                }
            ]
        }
    },
    computed: {
        address() {
            return this.shen + this.shi + this.qu + this.detail
        },
    },
    methods: {
        onClose() {
            this.$bus.$emit('on-close')
        },
        onConfirm() {
            if (!this.username.trim()) return alert('输入不能为空')
            const userobj = { id: nanoid(), username: this.username, userage: this.userage, sex: this.usersex, phone: this.phoneNumber, address: this.address, done: false }
            this.$bus.$emit('on-confirm', userobj)
            this.username = ''
            this.userage = ''
            this.usersex = ''
            this.phoneNumber = ''
            this.shen = ''
            this.shi = ''
            this.qu = ''
            this.detail = ''
        }
    },
    // mounted() {
    //     const node = document.createElement('div')
    //     node.id = 'CreateUser'
    //     document.body.appendChild(node)
    //     this.$once('hook:beforeDestroy', () => {
    //         document.body.removeChild(node)
    //     })
    // },
}
</script>

<style scoped>
.mask {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .3);
}

.createUserBox {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 60%;
    height: 500px;
    background-color: #fff;
}

.createUserBox h3 {
    text-align: center;
    margin-top: 5px;
}

.CreateUser-body {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 30% 30%;
    width: 90%;
    height: 80%;
    margin: 15px auto;
}

.CreateUser-footer {
    display: flex;
    flex-direction: row-reverse;
    padding-right: 50px;
}

.CreateUser-footer button {
    padding: 5px 25px;
    border: 0;
    cursor: pointer;
}

.CreateUser-footer button:nth-child(1) {
    margin-left: 5px;
    background-color: skyblue;
}

.CreateUser-footer button:nth-child(1):hover {
    color: #fff;
}

input,
select {
    display: inline-block;
    width: 90%;
    height: 28%;
    border-radius: 5px;
    outline: none;
    border: 1px solid #ccc;
    margin-top: 5px;
    padding-left: 5px;
}

.item:nth-child(5) .flexbox {
    display: flex;
}
</style>