<template>
    <div id="app">
        <div class="header-top">
            <Header></Header>
            <AddTodoItem
                v-on:addItem = "addItem"
                v-on:editItemEventUpdate = "editItemEventUpdate"
                :edit_item = "edit_item"
            ></AddTodoItem>
        </div>
        <div>
            <Todos
                :todo_items= "todo_items"
                v-on:deleteItemEvent= "deleteItemEvent"
                v-on:editItemEvent= "editItemEvent"
                v-on:checkedItemEvent= "checkedItemEvent">
            </Todos>
        </div>
    </div>
</template>

<script>
    import Header from "./components/Header";
    import AddTodoItem from "./components/AddTodoItem"
    import Todos from "./components/Todos"

    export default {
        name: "app",
        components: {
            Header,
            AddTodoItem,
            Todos
        },
        data() {
            return {
                todo_items: [],
                edit_item: {
                    id: "",
                    todo: "",
                    createdOn: "",
                    updatedOn: ""
                }
            }
        },
        methods: {
            addItem(newTodoItem) {
                this.todo_items.unshift(newTodoItem);

                const checked = this.todo_items.filter(todo_item => todo_item.checked === false);
                console.log(checked);
            },
            deleteItemEvent(id) {
                this.todo_items = this.todo_items.filter(todo_item => todo_item.id !== id)
            },
            editItemEvent(id) {
                const index = this.todo_items.findIndex(todo_item => todo_item.id === id);

                this.edit_item.todo = this.todo_items[index].todo;
                this.edit_item.createdOn = this.todo_items[index].createdOn;
                this.edit_item.id = id;
            },
            editItemEventUpdate(editedItem) {
                const index = this.todo_items.findIndex(todo_item => todo_item.id === editedItem.id);

                this.todo_items[index].todo = editedItem.todo;
                this.todo_items[index].updatedOn = editedItem.updatedOn;
            },
            checkedItemEvent(id) {
                const index = this.todo_items.findIndex(todo_item => todo_item.id === id);

                this.todo_items[index].checked = !this.todo_items[index].checked;
                this.todo_items[index].completedOn = this.getNow();
            },
            getNow() {
                const today = new Date();
                const date = today.getFullYear() + "-" + (today.getMonth() + 1) + "-" + today.getDate();
                const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();

                const now = date + " " + time;
                return now;
            }
        },
        mounted() {
            if(localStorage.getItem("todo_items")) {
                this.todo_items = JSON.parse(localStorage.getItem("todo_items"))
            }
        },
        watch: {
            todo_items: {
                handler() {
                    localStorage.setItem("todo_items", JSON.stringify(this.todo_items))
                },
                deep: true
            }
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        width: 60%;
        margin: 60px auto;
        background-color: #d8f3dc;
        padding: 45px 60px;
    }

    .header-top{
        position: sticky;
        top: 30px;
        background-color: #b7e4c7;
        padding: 30px 0;
        box-shadow: 0px 0px 20px 3px rgba(8, 28, 21,0.5);
    }

    * {
        box-sizing: border-box;
    }

    @media screen and (max-width: 1100px) {
        #app {
            width: 85%;
            margin: auto;
        }
    }

    @media screen and (max-width: 800px) {
        #app {
            width: 100%;
            margin: auto;
        }
    }
</style>