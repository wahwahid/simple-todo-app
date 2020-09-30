<template>
    <div class="add-todo-item">
        <link 
            rel="stylesheet" 
            href="https://use.fontawesome.com/releases/v5.2.0/css/all.css" 
            crossorigin="anonymous">

        <form>
            <input class="input" :class="{ edit: this.edit }" type="text" v-model="todo">
            <button 
                @click="submitValue"
                v-if="!this.edit"
                class="btn-add">
                <i class="fas fa-plus-circle"></i> Add
            </button>
            <button
                @click="editValue" 
                v-else 
                class="btn-pen-add">
                <i class="fas fa-upload"></i> Update
            </button>
        </form>
        
    </div>
</template>

<script>
    import { v4 as uuidv4 } from "uuid";

    export default {
        name: "AddTodoItem",
        props: ["edit_item"],
        data() {
            return {
                id: "",
                todo: "",
                edit: false,
                createdOn: "",
                updatedOn: ""
            }
        },
        methods: { 
            submitValue(e) {
                e.preventDefault();
                if(this.todo) {
                    const now = this.getNow();
                    const newTodo = {
                        id: uuidv4(),
                        todo: this.todo,
                        checked: false,
                        createdOn: now,
                        updatedOn: false
                    }

                    this.$emit("addItem", newTodo)
                    this.todo = "";
                }
            },
            editValue(e) {
                e.preventDefault();
                if(this.edit) {
                    const now = this.getNow();
                    const editTodo = {
                        id: this.id,
                        todo: this.todo,
                        checked: false,
                        createdOn: this.createdOn,
                        updatedOn: now
                    }

                    this.$emit("editItemEventUpdate", editTodo);
                    this.todo = "";
                    this.edit = false;
                }
            },
            getNow() {
                const today = new Date();
                const date = today.getFullYear() + "-" + (today.getMonth() + 1) + "-" + today.getDate();
                const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();

                const now = date + " " + time;
                return now;
            }
        },
        watch: {
            todo: {
                handler() {
                    if(!this.todo){
                        this.edit = false
                    }
                }
            },
            edit_item: {
                handler() {
                    this.id = this.edit_item.id;
                    this.todo = this.edit_item.todo;
                    this.edit = true,
                    this.createdOn = this.edit_item.createdOn
                },
                deep: true
            }
        }
    }
    
</script>

<style>
    .add-todo-item {
        width: 100%;
    }

    .add-todo-item form {
        width: 65%;
        display: flex;
        margin: auto;
    }

    .input {
        border: 1px solid #2e7718;
        padding: 12px 16px;
        border-radius: 20px 0 0 20px;
        width: 70%;
    }

    .edit {
        border: 1px solid DodgerBlue;
        padding: 12px 16px;
        border-radius: 20px 0 0 20px;
        width: 70%;
    }

    .btn-add {
        background-color: #2e7718; 
        border: none; 
        color: white; 
        padding: 10px 16px 13px 16px; 
        font-size: 16px; 
        cursor: pointer;
        border-radius: 0 20px 20px 0;
        width: 30%;
    }

    .btn-add:hover {
        background-color: #4fcc28;
    }

    .btn-pen-add {
        background-color: DodgerBlue;
        color: white;
        border: none;
        padding: 10px 16px 13px 16px; 
        font-size: 16px; 
        cursor: pointer;
        border-radius: 0 20px 20px 0;
        width: 30%;
    }
    
    .btn-pen-add:hover {
        background-color: RoyalBlue;
    }

    * {
        box-sizing: border-box;
    }

    @media screen and (max-width: 1280px) {
        .btn-add, .btn-pen-add, .edit{
            width: 40%;
        }

        .input {
            width: 60%;
        }
    }

    @media screen and (max-width: 630px){
        .add-todo-item form {
            width: 50%;
            display: block;
            margin: auto;
        }

        .btn-add, .btn-pen-add, .edit{
            width: 100%;
            border-radius: 0 0 20px 20px;
        }

        .input {
            width: 100%;
            border-radius: 20px 20px 0 0;
        }
    }
</style>