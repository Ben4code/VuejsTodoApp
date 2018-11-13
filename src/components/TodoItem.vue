<template>
  <div class="todoItem">
    <div class="todo-item-left">
        
        <input type="checkbox" v-model="completed" @change="doneEdit">

        <div :class="{strike: completed}" v-if="!editing" @dblclick="editTodo" class="todo-item-label">{{title}}</div>
        
        <input v-else type="text" v-model="title" class="todo-item-edit" @blur="doneEdit" @keyup.enter="doneEdit" v-focus @keypress.escape="cancelEdit">

        <div class="remove-todo" @click="removeTodo(index)"> &times; </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    todo:{
      type: Object,
      required: true,
    },
    index:{ 
      type: Number,
      reduired: true
    },
    checkAll: {
      type: Boolean,
      required: true
    }
  },
  watch:{
    checkAll(){
      if(this.checkAll){
        this.completed = true;
      }else{
        this.completed = this.todo.completed;
      }
    }
  },
  data(){
    return{
      'id': this.todo.id,
      'title': this.todo.title,
      'completed': this.todo.completed,
      'editing': this.todo.editing,
      'beforeEditCache': '' 
    }
  },
  methods:{
    removeTodo(index){
      eventBus.$emit('removedTodo', index);
    },
    editTodo(){
        this.beforeEditCache = this.title;
        this.editing = true;
    },
    cancelEdit(){
            this.title = this.beforeEditCache;
            this.editing = false;
        },
    doneEdit(){
        if(this.title.trim() === ''){
            this.title = this.beforeEditCache;
        }
        this.editing = false;
        eventBus.$emit('finishedEdit', {
        'index': this.index, 
        'todo':{
          'id': this.id, 
          'title': this.title, 
          'completed': this.completed, 
          'editing': this.editing
          }
        })
    },
    checkAllItem(){
      this.$emit('checkAllItem');
    }
  },
  directives:{
        focus: {
            inserted: function(el){
                el.focus();
            }
        },
    },
    
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
