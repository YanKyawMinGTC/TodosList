<template >
    <div class="container" >
        <h1>Todos List</h1>
        <input type="text" class="todo-input" placeholder="What needs to be done?" v-model="newtodo" @keyup.enter="addtodo">
        {{ noTodoslist ? " No Todo lists " : ""}}
            <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
            <div v-for="(todo, index) in this.todosFilter" :key="todo.id" class="to-do-item">
                <div class="to-do-item-left">
                    <input type="checkbox" @change="checkTodoslist" v-model="todo.completed">
                    <div v-if="!todo.editing" @dblclick="editTodo(todo)" :class="{ todotitle : todo.completed }">{{ todo.title }}</div>
                    <input type="text" v-else v-model="todo.title" @keyup.enter="doneEdit(todo)" @blur="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
                </div>
                <div class="remove-item" @click="removeTodo(index)">
                    &times;
                </div>
            </div>
            </transition-group>
            <hr>
            <div class="checkall-complete">
                <label :disabled="disableFlag"><input type="checkbox" :disabled="disableFlag" @change="checkAllTodos" v-model="allcheck">
                {{ checkAllFlag ? "UncheckAll" : "checkAll" }}</label>
                <div>
                {{ completeItems }} Items completed, {{NotCompleteItems}} Items not complete.
                </div>
            </div>
            <hr>
            <div class="clear-all">
                <div>
                    <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
                    <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
                    <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
                </div>
                <button v-if="clearComplete" @click="clearCompleted()">Clear Completed Tasks</button>
            </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            newtodo : '',
            nextId : 3,
            editCache : '',
            allcheck : false,
            disableFlag : false,
            filter : 'all',
            todos : [
                {
                    'id' : 1,
                    'title' : 'Move the world!',
                    'completed' : true,
                    'editing' : false,
                },
                {
                    'id' : 2,
                    'title' : 'Meet you!',
                    'completed' : true,
                    'editing' : false,
                },
            ]
        }
    },
    methods: {
        addtodo(){
            if(this.newtodo.trim().length == 0){
                return
            }
            this.todos.push({
                'id' : this.nextId,
                'title': this.newtodo,
                'completed' : false,
            })
            this.newtodo = ''
            this.nextId ++
            this.disableFun()
            this.checkTodoslist()
        },
        editTodo(item){
            this.editCache = item.title
            return item.editing = true
        },
        doneEdit(item){
            if(item.title.trim().length == 0){
                item.title = this.editCache
            }
            this.disableFun()
            return item.editing = false
        },
        cancelEdit(item){
            item.title = this.editCache
            return item.editing = false
        },
        removeTodo(index){
            this.todos.splice(index, 1)
            this.disableFun()
            this.checkTodoslist()
        },
        checkAllTodos(){
            this.todos.forEach(todo => todo.completed = event.target.checked)
        },
        clearCompleted(){
            this.todos = this.todos.filter(todo=>todo.completed != true)
            this.disableFun()
            this.checkTodoslist()
        },
        disableFun(){
            this.todos.length < 1 ? this.disableFlag = true : this.disableFlag = false
        },
        checkTodoslist(){
            if(this.todos.length > 0 && this.todos.every(todo=>todo.completed == true)){
                this.allcheck = true
            }
            else if(this.todos.length > 0 && this.todos.every(todo=>todo.completed == false)){
                this.allcheck = false
            }else{
                this.allcheck = false
            }
        },
    },
    computed:{
        todosFilter(){
            if(this.filter == 'all'){
                return this.todos
            }else if(this.filter == 'active'){
                return this.todos.filter(todo => todo.completed == false)
            }else if(this.filter == 'completed'){
                return this.todos.filter(todo => todo.completed == true)
            }else{
                return this.todos
            }
        },
        checkAllFlag(){
            return this.todos.length > 0 && this.todos.every(todo=>todo.completed == true)
        },
        completeItems(){
            return this.todos.filter(todo=>todo.completed != false).length
        },
        NotCompleteItems(){
            return this.todos.filter(todo=>todo.completed != true).length
        },
        noTodoslist(){
            return this.todos.length == 0
        },
        clearComplete(){
            return this.todos.length > 0 && this.todos.some(todo=>todo.completed == true)
        }
    },
    directives: {
        focus: {
            // directive definition
            inserted: function (el) {
                console.log(el)
            el.focus()
            }
        }
    },
    mounted(){
        this.disableFun()
        this.checkTodoslist()
    }
}
</script>
<style lang='scss'>
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom : 16px;
    &:focus {
        outline: none;
    }
}
.to-do-item, .clear-all {
    display: flex;
    align-items : center;
    margin-bottom : 12px;
    justify-content: space-between;
    animation-duration: 0.3s;
}
.to-do-item-left {
    display: flex;
    
}
.checkall-complete {
    display: flex;
    margin-bottom : 12px;
    justify-content: space-between;
}
.remove-item {
    cursor: pointer;
    margin-left : 14px;
    &:hover {
        color: black;
    }
}
.todotitle {
    text-decoration: line-through;
}
button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    border: 1px solid grey;

    &:hover {
        background: #6224d4;
    }

    &:focus {
        outline: none;
    }
}

.active {
    background: #6224d4;
}

// CSS Transitions
.fade-enter-active, .fade-leave-active {
transition: opacity .2s;
}

.fade-enter, .fade-leave-to {
opacity: 0;
}

</style>