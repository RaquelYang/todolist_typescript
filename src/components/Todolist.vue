<template>
  <div class="todolist">
    <h3>Todolist</h3>
    <div class="input-group mb-3">
      <input type="text" class="form-control"
        placeholder="Todo" aria-label="Todo" aria-describedby="button-addon2"
        v-model="todoInput" @keydown.enter="addTodo()">
      <div class="input-group-append">
        <button class="btn btn-outline-secondary" type="button" id="button-addon2" @click="addTodo()">ADD</button>
      </div>
    </div>
    <ul class="list-group">
      <li v-for="(todo,index) in todoList" 
        :key="index"
        class="list-group-item"
        :class="{
          'list-group-item':true,
          'list-group-item-secondary':todo.checked
        }"
        @click="toggleChecked(index)">
        {{ todo.todo }}
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"
          @click.stop="delTodo(todo)"
        >
          <span aria-hidden="true">&times;</span>
        </button>
        <button type="button" class="close mr-5" data-dismiss="modal" aria-label="Close"
          @click.stop="editTodo(todo)"
        >
          <span aria-hidden="true">&#200;</span>
        </button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

class Todo {
  todo: string;
  checked: boolean;
  constructor(todo: string) {
    this.todo = todo;
    this.checked = false;
  }
  
}

@Component
export default class Todolist extends Vue {
  todoInput: string = '';
  todoList: Todo[] = [];

  toggleChecked(index: number) {
    this.todoList[index].checked = !this.todoList[index].checked
  }

  save(){
    window.localStorage.setItem('typeScriptTodos', JSON.stringify(this.todoList))
  }

  addTodo(){
    console.log('TodoInput',this.todoInput);
    let todo = new Todo(this.todoInput)
    this.todoList.push(todo)
    this.todoInput = ''
    this.save()
  }

  delTodo(todo:Todo){
    console.log('delTodo',todo);
    this.todoList = this.todoList.filter((todoItem)=>{
      return todoItem !== todo;
    })
    this.save()
  }
  editTodo(todo:Todo){
    console.log(todo);
    let content = window.prompt('請輸入修改內容')
    console.log(content);
    if(content?.trim()){
      todo.todo = content.trim()
      this.save()
    }
  }
  mounted(){
    const store = window.localStorage.getItem('typeScriptTodos')
    if(store!==null){
      this.todoList = JSON.parse(store) || []
    }
  }
}
</script>

<style scoped>
.list-group-item-secondary{
  text-decoration: line-through;
}
</style>
