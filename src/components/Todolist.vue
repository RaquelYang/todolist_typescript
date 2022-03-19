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
        <button type="button" class="close mr-2" data-dismiss="modal" aria-label="Close"
          @click.stop="editTodo(todo)"
        >
          <span aria-hidden="true">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
              <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
            </svg>
          </span>
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
    this.todoList[index].checked = !this.todoList[index].checked;
  }

  save(){
    window.localStorage.setItem('typeScriptTodos', JSON.stringify(this.todoList));
  }

  addTodo(){
    // console.log('TodoInput',this.todoInput);
    let todo = new Todo(this.todoInput);
    this.todoList.push(todo);
    this.todoInput = '';
    this.save();
  }

  delTodo(todo: Todo) {
    // console.log('delTodo',todo);
    this.todoList = this.todoList.filter((todoItem)=>{
      return todoItem !== todo;
    })
    this.save();
  }
  editTodo(todo: Todo) {
    // console.log(todo);
    const content = window.prompt('請輸入修改內容')
    // console.log(content);
    if (content?.trim()) {
      todo.todo = content.trim();
      this.save();
    }
  }
  mounted() {
    const store = window.localStorage.getItem('typeScriptTodos');
    if (store !== null) {
      this.todoList = JSON.parse(store) || [];
    }
  }
}
</script>

<style scoped>
.list-group-item-secondary{
  text-decoration: line-through;
}
</style>
