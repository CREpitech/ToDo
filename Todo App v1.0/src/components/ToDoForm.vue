<template>
    <form @submit.prevent='onSubmit'>
        
        <h2 class="label-wrapper">
            <label for="new to-do-input" class="label__lg">
            What do you need "To Do" next?
            </label>
        </h2>
        <input 
        type="text"
        id="new to-do-input"
        name="new-todo"
        autocomplete="off"
        v-model.lazy.trim="label"
        class="input__lg"
        />
        <button type="submit" class="btn btn__primary btn__lg">
            +
        </button>
    </form>
</template>
<!-- ToDoForm.vue

<form> listens for submit event. Result: onSubmit() method is invoked, which checks that the new label is not empty, then emits

 the todo-added event (which is then listened for inside App.vue, see above), and finally clears the new label <input>. -->
<script>
export default {
    methods: {
        onSubmit() {
            if(this.label === ""){
                return;
            }
            this.$emit('todo-added', this.label);
            this.label="";
            /* console.log("New ToDo Submmited "+ "Label value: " + this.label); */
        }
    },
    data(){
        return {
            label:""
        };
    }
};
</script>