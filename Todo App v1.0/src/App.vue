<template>
  <h1>ToDo App</h1>
  <!-- <h2>{{ToDoItems.filter(item => item.done).length}} out of {{ToDoItems.length}} items completed</h2> -->

  <div>
    <to-do-form @todo-added="addToDo"></to-do-form>
  </div>
  
  <div>
     <h2 id="list-summary" ref='listSummary' tabIndex="-1">{{listSummary}}</h2>
    <div aria-labelledby="list-summary" class="stack-large">
      <div v-for="item in TodoItems" :key="item.id">
        <todo-item :label="item.label" :done="item.done" :id="item.id" @checkbox-changed="updateDoneStatus(item.id)" @item-deleted="deleteToDo(item.id)"
        @item-edited="editToDo(item.id, $event)"></todo-item>
      </div>
    </div>
    
  </div>
</template>
<!--App.vue

<to-do-form> listens for:

    todo-added event emitted by the onSubmit() method inside the ToDoForm component when the form is submitted. Result: addToDo() method invoked to add new todo item to the ToDoItems array.

<to-do-item> listens for:

    checkbox-changed event emitted by the checkbox <input> inside the ToDoItem component when it is checked or unchecked. Result: updateDoneStatus() method invoked to update done status of associated todo item.
    item-deleted event emitted by the deleteToDo() method inside the ToDoItem component when the "Delete" button is pressed. Result: deleteToDo() method invoked to delete associated todo item.
    item-edited event emitted by the itemEdited() method inside the ToDoItem component when the item-edited event emitted by the onSubmit() method inside the ToDoItemEditForm has been successfully listened for. Yes, this is a chain of two different item-edit events! Result: editToDo() method invoked to update label of associated todo item.
 -->
<script>
import TodoItem from "./components/TodoItem.vue";
import uniqueId from "lodash.uniqueid";
import ToDoForm from './components/ToDoForm.vue'

export default {
  name: "App",
  components: {
    TodoItem,
    ToDoForm,
  },
  data() {
    return {
      TodoItems: [
        { id: uniqueId("todo-"), label: "Learn Vue.js", done: false },
        { id: uniqueId("todo-"), label: "aaa", done: false },
        { id: uniqueId("todo-"), label: "ride more bike", done: false },
        { id: uniqueId("todo-"), label: "Study for Life", done: false },
      ],
    };
  },
  mounted(){
    if(localStorage.TodoItems){
      this.TodoItems = JSON.parse(localStorage.TodoItems)
      
      console.log("there are items in localstorage func so that if return true")
    } else {
      console.log("au contraire")
      localStorage.setItem('Items', this.TodoItems )
    }
   
  },
  watch:{
    TodoItems:{
      handler(addToDo){
        localStorage.TodoItems = JSON.stringify(addToDo)
      },
      deep: true

    }
  },
  
  methods:{
    addToDo(toDoLabel){
      
      this.TodoItems.push({id:uniqueId('todo-'), label:toDoLabel, done:false});
      /* console.log("New ToDo Submmited " + toDoLabel) */
    },
    updateDoneStatus(toDoId){
      const toDoUpdate = this.TodoItems.find(item => item.id === toDoId)
      toDoUpdate.done = !toDoUpdate.done
    },
    deleteToDo(toDoId){
       const itemIndex = this.TodoItems.findIndex(item => item.id === toDoId);
       this.TodoItems.splice(itemIndex, 1);
       this.$refs.listSummary.focus();
   },
   editToDo(toDoId, newLabel){
     const toDoToEdit = this.TodoItems.find(item => item.id === toDoId);
     toDoToEdit.label = newLabel;
   }
  },
  computed: {
    listSummary() {
      const numberFinishedItems = this.TodoItems.filter(item =>item.done).length
      return `${numberFinishedItems} out of ${this.TodoItems.length} items completed`
    }
  }
};
</script>

<style>
/*Global Styles*/
.btn {
  margin-top:100px;
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}
.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}
.btn__filter {
  border-color: lightgrey;
}
.btn__danger:focus {
  outline-color: #c82333;
}
.btn__primary {
  color: #fff;
  background-color: #000;
}
.btn-group {
  display: flex;
  justify-content: space-between;
}
.btn-group > * {
  flex: 1 1 auto;
}
.btn-group > * + * {
  margin-left: 0.8rem;
}
.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}
[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}
[class*="__lg"]:not(:last-child){
  margin-bottom: 1rem;
}
@media screen and (min-width: 620px) {
  [class*="__lg"]{
    font-size: 2.4rem;
  }
}
.visually-hidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px 1px 1px 1px);
  clip-path: rect(1px 1px 1px 1px);
  white-space: nowrap;

}
[class*="stack"] > * {
  margin-top: 0;
  margin-bottom: 0;
}
.stack-large {
  margin: 0 auto;
  max-width: 1000px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(225px, 1fr));
  gap: 20px;
  /* align-content: center; */
  justify-content: center;
}
.stack-small {
  height: 200px;
  width: 200px;
  align-self: center;
  background-color: yellow;
  
}
.stack-small:hover{
 box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.2)
}
.stack-small > * + * {
  margin-top: 1.25rem;
}
.stack-large > * + * {
  margin-top: 2.5rem;
}
@media screen and (min-width: 550px){
  .stack-small > * + * {
  margin-top: 1.4rem;
}
.stack-large > * + * {
  margin-top: 2.8rem;
}
}
/*END OF CUSTOM GLOBAL STYLES*/
#app{
  background:rgb(248, 248, 245);
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.2), 0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}
@media screen and (min-width: 550px){
  #app{
    padding: 4rem;
  }
}
#app > * {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}
#app > form {
  max-width: 100%;
}
#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin:0;
  margin-bottom: 1rem;
}
/*Next Comment is the Vue's Default style for div id="App" i'll leave it ther, but we can delete 
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
<!--//template   <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/> //script  import HelloWorld from './components/HelloWorld.vue'  //script.export  /*  components: {
    HelloWorld
  } */ -->
  