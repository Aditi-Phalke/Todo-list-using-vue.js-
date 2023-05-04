<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([])
const name = ref('')

const input_content = ref('')

const todos_asc = computed(()=> todos.value.sort((a,b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if(input_content.value.trim()=== ''){
    return 
  }
todos.value.push({
  content: input_content.value,
  done: false,
  createdAt: new Date().getTime()
})
input_content.value = ''
}

const removeTodo= todo=>{
  todos.value = todos.value.filter(t => t!== todo)
}

watch(todos,newVal=>{
  localStorage.setItem('todos',JSON.stringify(newVal))
},{ deep: true})

watch(name,(newVal)=>{
  localStorage.setItem('name',newVal)
})

onMounted(()=>{
  name.value=localStorage.getItem('name')||''
  todos.value=JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <div class="container" style="max-width: 600px;">
 <main class="app">
  <section class="greeting">
    <h2 class="title">
      What's up, <input type="text" placeholder="Name here"
      v-model="name" />
     
    </h2>
  </section>

  <section class="create-todo">
    <h3>CREATE A TODO</h3>

    <form @submit.prevent="addTodo">
      <h3>What's on your todo list?</h3>
      <input type="text" placeholder="e.g. make a video" v-model="input_content" />
      <input type="submit" value="Add todo"/>
    </form>
  </section>

  <section class="todo-list">
    <h3>TODO LIST</h3>
    <div class="list">
      <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
        <label>
          <input type="checkbox" v-model="todo.done" />
        </label>
        <div class="todo-content">
          <input type="text" v-model="todo.content" />
          
        </div>

        <div class="actions">
          <button class="delete" @click="removeTodo(todo)">Delete</button>

        </div>
      </div>
    </div>
  </section>
  <div>
   
    <button v-on:click="showInputBox = true" v-if="!showInputBox"><h3>📁 Add new folder</h3></button>
    <div v-if="showInputBox">
      <input type="text" placeholder="Enter the name of folder" v-model="newFolderName">
          <button type="submit" @click="createFolder"><h3>Create folder</h3></button>
    </div>
  </div>
 </main>
</div> 
</template>

<script>
export default {
  data() {
    return {
      showInputBox: false,
      newFolderName: ''
    };
  },
  methods: {
    createFolder() {
      var folder = {
        name: this.newFolderName,
        id: Date.now(),
        todos: []
      };

      this.folders.push(folder);

      this.newFolderName = '';
      this.showInputBox = false;
    }
  }
};
</script>













