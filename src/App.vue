<template>
  <div id="app">
    <h4>用户管理</h4>
    <div class="container-box">
      <div class="button-group">
        <button @click="ejectWindow">新建</button>
        <input type="text" placeholder="按关键字搜索" v-model="keyWord" @input="somemethods">
        <button id="back">撤销</button>
      </div>
      <user-list :UserDatas="showData"></user-list>
      <button class="deletebutton" @click="clearAll">批量删除</button>
    </div>
    <create-user :visible="UserCreateIsvisible"></create-user>
    <edit-user :OneData="OneData" v-if="flag"></edit-user>
  </div>
</template>

<script>
import CreateUser from './components/CreateUser'
import UserList from './components/UserList'
import EditUser from './components/EditUser'

export default {
  name: 'App',
  components: {
    CreateUser,
    UserList,
    EditUser
  },
  data() {
    return {
      UserCreateIsvisible: false,
      flag: false,
      UserDatas: JSON.parse(localStorage.getItem('UserDatas')) || [],
      OneData: [],
      showData: JSON.parse(localStorage.getItem('UserDatas')) || [],
      keyWord: ''
    }
  },
  methods: {
    somemethods() {
      if (this.keyWord === '') {
        this.showData = this.UserDatas
      } else {
        this.showData = this.UserDatas.filter((item) => {
          return item.username === this.keyWord
        })
      }
    },
    ejectWindow() {
      this.UserCreateIsvisible = true
    },
    closeUserCreate() {
      this.UserCreateIsvisible = false
    },
    closeEditUserCreate() {
      this.flag = false
    },
    confirmUserCreate(userobj) {
      // console.log(userobj);
      this.UserDatas.unshift(userobj)
      this.showData = this.UserDatas
      this.UserCreateIsvisible = false
    },
    deleteUserData(id) {
      this.UserDatas = this.UserDatas.filter(item => item.id !== id)
      this.showData = this.UserDatas
    },
    checkUserData(id) {
      this.UserDatas.forEach((item) => {
        if (item.id === id) item.done = !item.done
      })
    },
    clearAll() {
      this.UserDatas = this.UserDatas.filter((item) => {
        return !item.done
      })
      this.showData = this.UserDatas
    },
    editUserData(id) {
      this.OneData = this.UserDatas.filter((item) => {
        return item.id === id
      })
      this.flag = true
      // console.log(this.OneData);
    },
    editUserCreate(userobj) {
      this.UserDatas.forEach((item, index) => {
        if (item.id === userobj.id) {
          this.UserDatas[index] = userobj
          localStorage.setItem('UserDatas', JSON.stringify(this.UserDatas))
        }
      })
      this.showData = this.UserDatas
      // console.log(this.showData);
      this.flag = false
      document.location.reload()
    }
  },
  watch: {
    UserDatas: {
      deep: true,
      handler(value) {
        localStorage.setItem('UserDatas', JSON.stringify(value))
        this.showData = this.UserDatas
      }
    }
  },
  mounted() {
    this.$bus.$on('on-close', this.closeUserCreate)
    this.$bus.$on('on-close2', this.closeEditUserCreate)
    this.$bus.$on('on-confirm', this.confirmUserCreate)
    this.$bus.$on('on-confirm2', this.editUserCreate)
    this.$bus.$on('deleteUserData', this.deleteUserData)
    this.$bus.$on('checkUserData', this.checkUserData)
    this.$bus.$on('editUserData', this.editUserData)
  },
  beforeDestroy() {
    this.$bus.$off('on-close')
    this.$bus.$off('on-close2')
    this.$bus.$off('on-confirm')
    this.$bus.$off('on-confirm2')
    this.$bus.$off('deleteUserData')
    this.$bus.$off('checkUserData')
    this.$bus.$off('editUserData')
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

ul {
  list-style: none;
}

#app h4 {
  height: 80px;
  line-height: 80px;
  text-align: center;
  background-color: rgb(228, 228, 228);
  font-size: 18px;
}

.container-box {
  width: 85%;
  margin: 40px auto;
}

.container-box .button-group button,
.deletebutton {
  background-color: rgb(228, 228, 228);
  padding: 5px 30px;
  border: 0;
  cursor: pointer;
  margin-top: 10px;
}

.container-box .button-group input {
  outline: none;
  border: 1px solid transparent;
  background-color: rgb(228, 228, 228);
  padding: 5px 10px;
  margin-left: 10px;
}

.container-box .button-group input::placeholder {
  color: black;
}

#back {
  float: right;
}
</style>
