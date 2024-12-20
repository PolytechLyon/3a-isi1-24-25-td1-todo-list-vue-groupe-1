<script setup>
import { ref } from 'vue'

defineProps(
    { msg: String, },

)

const txtlist = ref([])
const editingid = ref(0)

const addItem = () => {
    const title = document.getElementById('new-todo-item-title').value
    if (title) {
        txtlist.value.push({ id: txtlist.value.length + 1, title })
        document.getElementById('new-todo-item-title').value = ''
    }
    console.log('Item '+ txtlist.value.length +' added')
}

const deleteItem = (id) => {
    txtlist.value.splice(txtlist.value.findIndex(item => item.id === id), 1)
    console.log('Item '+ id +' removed')
}

const edit = () => {
    console.log('Edit mode opened for item '+ editingid.value)
    document.getElementById('edit-item').hidden = false
    document.getElementById('edit-todo-item-title').value = txtlist.value[txtlist.value.findIndex(item => item.id === editingid.value)].title
}

const editItem = () => {
    const title = document.getElementById('edit-todo-item-title').value
    if (title) {
        txtlist.value.splice(txtlist.value.findIndex(item => item.id === editingid.value), 1)
        txtlist.value.push({ id: editingid.value, title:title })
        document.getElementById('edit-todo-item-title').value = ''
        console.log('Item '+ editingid.value +' modified')
    }
    document.getElementById('edit-item').hidden = true
    editingid.value=0
}

const cancel = () => {
    document.getElementById('edit-todo-item-title').value = ''
    document.getElementById('edit-item').hidden = true
    console.log('Item '+ editingid.value +' unmodified')
    editingid.value=0
}

</script>

<template>
    <h1>{{ msg }}</h1>

    <div id="new-item">
        <h2>New todo item</h2>
        <label for="new-todo-item-title">Title : </label>
        <input id="new-todo-item-title" />
        <button v-on:click="addItem">Add</button>
    </div>

    <div id="todo_list">
        <h2>Todo items</h2>
        <ul id="todo-list">
            <li v-for="item in txtlist" :key="item.id">
                <span>{{ item.title }}</span>
                <button v-on:click="editingid=item.id; edit()">Edit</button>
                <button v-on:click="deleteItem(item.id)">Delete</button>
            </li>
        </ul>
    </div>

    <div id="edit-item" hidden>
        <h2>Edit todo item</h2>
        <label for="edit-todo-item-title">Title : </label>
        <input id="edit-todo-item-title" />
        <button v-on:click="editItem()">Confirm</button>
        <button v-on:click="cancel">Cancel</button>
    </div>
</template>

<style scoped>
.read-the-docs {
    color: #888;
}

#todo-list {
    li {
        span {
            display: inline-block;
            width: 15em;
        }

        button {
            width: 4em;
            margin: 2px;
        }
    }
}
</style>
