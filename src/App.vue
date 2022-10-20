<template>
  <div class="container">
    <div class="row mt-3">
      <h3 class=" border-bottom col-md-8 offset-md-2 text-center bg-light p-3 text-success">To Do App | Vue.js</h3>
    </div>
    <div class="row">
      <div class="col-md-8 offset-md-2 d-flex justify-content-between border-bottom align-items-center py-3">
        <input class="form-control me-3" type="text" v-model="todoText" placeholder="Entry a to do">
        <button @click="addTodo" class="btn btn-primary">Add</button>
        
      </div>
    </div>
     
    <Todo @deleteTodo="deleteTodo($event)" v-for="todo in todoList" :key="todo.id" :todo="todo"/>
  </div>
  
</template>

<script>
import Todo from "@/components/todo-comp"
import axios from "axios"
export default {
  components:{
    Todo
  },
  data(){
    return{
      todoText:"",
      todoList:[]
    }
  },
  methods:{
    addTodo(){
      axios.post("https://todoapp-58cdf-default-rtdb.firebaseio.com/todoList.json",{text : this.todoText})
      .then(response =>{
        this.todoList.push({
          id : response.data.name,
          text : this.todoText
        })
      })
      .catch(e=>{
        console.log(e)
      })
     },
     deleteTodo(todoId){
      axios.delete("https://todoapp-58cdf-default-rtdb.firebaseio.com/todoList/" +todoId+ ".json")
      .then(response=>{
        console.log(response)
        let index =this.todoList.findIndex(i=>{
          return i.id ==todoId
        })
        this.todoList.splice(index,1)
      })
      .catch(e=>{
        console.log(e)
      })
      }
     
  },
  created(){
      axios.get("https://todoapp-58cdf-default-rtdb.firebaseio.com/todoList.json")
      .then(response=>{
        
        for(let key in response.data){
          let todo={
            text : response.data[key].text,
            id : key
          }
          this.todoList.push(todo)
          
        }
      })
      .catch(e=>{ 
        console.log(e)
      })
     }
}
</script>

