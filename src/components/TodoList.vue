<template>
    <div>
        <p> Todo List goes here.</p>
        <input type="text" class="todo-input" placeholder="Write your todos." v-model="newTodo" @keyup.enter="addTodo">

        <transition-group enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">    
            <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index" @removedTodo="removeTodo" @finishedEdit="finishedEdit" :checkAll="!zeroRemaining">
            </todo-item>
        </transition-group>
        
        <div class="extra-container">
            <div><label><input type="checkbox" @click="checkAll" :checked="!zeroRemaining" >Check All</label></div>
            <div>{{remaining}} items left</div>
        </div>

        <div class="extra-container">
            <div>
                <button :class="{active: filter === 'all'}" @click="filter = 'all'">All</button>
                <button :class="{active: filter === 'active'}" @click="filter = 'active'">Active</button>
                <button :class="{active: filter === 'completed'}" @click="filter = 'completed'">Completed</button>
            </div>
            <div>
                <transition name="fade">
                     <button v-if="showClearCompleted" @click="clearCompleted"> Clear Completed</button>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
import TodoItem from './TodoItem';


export default {
    name: 'todo-list',
    components: {
        TodoItem
    },
    data(){
        return {
            beforeEditCache: '',
            filter: 'all',
            newTodo: '',
            todoId: null,
            todos: [
                {id: 1, title: 'Finish tutorial', completed: false, editing: false},
                {id: 2, title: 'Prepare dinner', completed: true, editing: false},
                {id: 3, title: 'Take bath', completed: false, editing: false}
            ]
        }
    },
    computed:{
        remaining(){
            const rem = this.todos.filter((todo)=>{
                return (!todo.completed);
            });
            return rem.length;
        },
        zeroRemaining(){
            return this.remaining !== 0;
        },
        todosFiltered(){
            if(this.filter == 'all'){
                return this.todos;
            }else if(this.filter === 'active'){
                return this.todos.filter(todo => !todo.completed);
            }else if(this.filter === 'completed'){
                return this.todos.filter(todo => todo.completed);
            }
            return this.todos;
        },

        showClearCompleted(){
           return this.todos.filter(todo => todo.completed).length > 0; 
        }
    },
    methods:{
        addTodo(){
            if(this.newTodo.trim().length !== 0){
                this.todos.push({id: this.todoId, title: this.newTodo, completed: false});
            }
            //Clear input
            this.newTodo = '';
            this.todoId++;
        },
        removeTodo(index){
            this.todos.splice(index, 1);
        },
        checkAll(){
            this.todos.forEach(todo => {
                todo.completed = event.target.checked;
            });
        },
        clearCompleted(){
           this.todos = this.todos.filter(todo => !todo.completed); 
        },
        finishedEdit(data){
            this.todos.splice(data.index, 1, data.todo)
        },
    },
    created(){
        eventBus.$on('removedTodo', (index) => this.removeTodo(index));
        eventBus.$on('finishedEdit', (data) => this.finishedEdit(data));
    }
}
</script>

<style lang = "scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.min.css");

    .todo-input{
        width: 100%;
        padding: 10px 10px;
        font-size: 18px;
        margin-bottom: 16px;

        &:focus{
            outline: 0;
        }
    }

    .todo-item{
        margin-bottom: 12px;
        animation-duration: 0.3s;
    }
    .remove-todo{
        cursor: pointer;
        color: #41B883;
        background: #35495E;
        padding: 2px 8px;
        border-radius: 50%;
    }
    .todo-item-left{
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .todo-item-label{
        padding: 10px;
        border: 1px solid #fff;
        margin-left: 12px;
    }
    .todo-item-edit{
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir';

        &:focus{
            outline:none
        }
    }
    .strike{
        text-decoration: line-through;
        color: grey;
    }
    .extra-container{
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top:  1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }
    button{
        font-size: 14px;
        background-color: #fff;
        appearance: none;
        border: 1px solid #ccc;
        cursor: pointer;
        margin-right: 12px;
        padding: 4px;

        &:hover{
            background: lightgreen;
        }

        &:focus{
            outline: none;
        }
    }
    .active{
        background:lightgreen;
    }


    /* CSS Transitions */
    .fade-enter-active, .fade-leave-active{
        transition: opacity .2s;
    }
    .fade-enter, .fade-leave-to{
         opacity: 0;
    }

</style>
