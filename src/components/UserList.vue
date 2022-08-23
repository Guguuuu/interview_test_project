<template>
    <table>
        <tr>
            <th></th>
            <th>姓名</th>
            <th>年龄</th>
            <th>性别</th>
            <th>联系电话</th>
            <th>详细地址</th>
            <th>操作</th>
        </tr>
        <UserItem v-for="a in UserDatas" :key="a.id" :UserData="a"></UserItem>
    </table>
</template>

<script>
import UserItem from './UserItem.vue';

export default {
    name: 'UserList',
    props: ['UserDatas'],
    components: { UserItem }
}
</script>

<style scoped>
table {
    width: 100%;
    margin-top: 10px;
    border: 2px solid rgb(228, 228, 228);
}

tr:first-child {
    background-color: rgb(228, 228, 228);
}

th {
    width: 10%;
    padding: 3px;
}

th:nth-child(6) {
    width: 45%;
}

th:first-child {
    width: 2%;
}
</style>