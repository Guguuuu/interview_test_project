<template>
    <tr>
        <td><input type="checkbox" :checked="UserData.done" @change="handleCheck(UserData.id)"></td>
        <td><span>{{ UserData.username }}</span></td>
        <td><span>{{ UserData.userage }}</span></td>
        <td><span>{{ UserData.sex }}</span></td>
        <td><span>{{ UserData.phone }}</span></td>
        <td><span>{{ UserData.address }}</span></td>
        <td>
            <button @click="handleEdit(UserData.id)">编辑</button>
            <button @click="handleDelete(UserData.id)">删除</button>
        </td>
    </tr>
</template>

<script>
export default {
    name: 'UserItem',
    props: ['UserData'],
    methods: {
        handleDelete(id) {
            if (confirm('确定删除吗？')) {
                this.$bus.$emit('deleteUserData', id)
            }
        },
        handleCheck(id) {
            //通知App组件将对应的UserData对象的done值取反
            this.$bus.$emit('checkUserData', id)
        },
        handleEdit(id) {
            this.$bus.$emit('editUserData', id)
        }
    },
}
</script>

<style scoped>
td {
    text-align: center;
}

td:last-child button {
    background-color: rgb(228, 228, 228);
    padding: 5px 17px;
    margin: 2px 2px;
    border: 0;
    cursor: pointer;
}
</style>