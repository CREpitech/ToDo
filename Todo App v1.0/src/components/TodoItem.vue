<template>
  <div class="stack-small" v-if="!isEditing">
    <div class="custom-checkbox">
      <input
        type="checkbox"
        :id="id"
        :checked="isDone"
        class="checkbox"
        @change="$emit('checkbox-changed')"
      />
      <label :for="id" class="checkbox-label">{{ label }}</label>
    </div>
    <div class="btn-group">
      <button
        type="button"
        class="btn"
        @click="toggleToItemEditForm"
        ref="editButton"
      >
        EDIT <span class="visually-hidden"></span>
      </button>
      <button type="button" class="btn btn__danger" @click="deleteToDo">
        DELETE<span class="visually-hidden">{{ label }}</span>
      </button>
    </div>
  </div>
  <to-do-item-edit-form
    v-else
    :id="id"
    :label="label"
    @item-edited="itemEdited"
    @edit-cancelled="editCancelled"
  ></to-do-item-edit-form>
</template>
<!--
ToDoItem.vue

checkbox <input> listens for change event. Result: checkbox-changed event emitted when the checkbox is checked/unchecked (which is 

then listened for inside App.vue; see above).

"Edit" <button> listens for click event. Result: toggleToItemEditForm() method is invoked, which toggles this.isEditing to true,

 which in turn displays the todo item's edit form on re-render.

"Delete" <button> listens for click event. Result: deleteToDo() method is invoked, which emits the item-deleted event (which is 

then listened for inside App.vue; see above)

<to-do-item-edit-form> listens for:

    item-edited event emitted by the onSubmit() method inside the ToDoItemEditForm component when the form is successfully 
    
    submitted. Result: itemEdited() method is invoked, which emits the item-edited event (which is then listened for inside App.
    
    vue, see above), and sets this.isEditing back to false, so that the edit form is no longer shown on re-render.

    edit-cancelled event emitted by the onCancel() method inside the ToDoItemEditForm component when the "Cancel"  button is 
    
    clicked. Result: editCancelled() method is invoked, which sets this.isEditing back to false, so that the edit form is no 
    
    longer shown on re-render.

-->
<script>
import ToDoItemEditForm from "./ToDoItemEditForm.vue";

export default {
  components: {
    ToDoItemEditForm,
  },
  props: {
    label: {
      required: true /* "true" */,
      type: String,
    },
    done: {
      default: false,
      type: Boolean,
    },
    id: {
      required: true,
      type: String,
    },
  },
  data() {
    return {
      isEditing: false,
    };
  },
  computed: {
    isDone() {
      return this.done;
    },
  },
  methods: {
    deleteToDo() {
      this.$emit("item-deleted");
    },
    toggleToItemEditForm() {
      console.log( this.$refs.editButton );
      this.isEditing = true;
    },
    itemEdited(newLabel) {
      this.$emit("item-edited", newLabel);
      this.isEditing = false;
      this.focusOnEditButton()
    },
    editCancelled() {
      this.isEditing = false;
      this.focusOnEditButton()
    },
    focusOnEditButton(){
      this.$nextTick(() =>{
        let editButtonRef= this.$refs.editButton;
        editButtonRef.focus();
      })
      
    }
  }  
};
</script>


<style scoped>
.custom-checkbox > .checkbox-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
.custom-checkbox > .checkbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  box-sizing: border-box;
  width: 100%;
  height: 40px;
  height: 2.5rem;
  margin-top: 0;
  padding: 5px;
  border: 2px solid #0b0c0c;
  border-radius: 0;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}
.custom-checkbox > input:focus {
  outline: 3px dashed #fd0;
  outline-offset: 0;
  box-shadow: inset 0 0 0 2px;
}
.custom-checkbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  font-weight: 400;
  font-size: 1.6rem;
  line-height: 1.25;
  display: block;
  position: relative;
  min-height: 10px;
  margin-bottom: 10px;
  padding-left: 40px;
  clear: left;
}
.custom-checkbox > input[type="checkbox"] {
  -webkit-font-smoothing: antialiased;
  cursor: pointer;
  position: absolute;
  z-index: 1;
  top: -2px;
  left: -2px;
  width: 14px;
  height: 14px;
  margin: 0;
  opacity: 0;
}
.custom-checkbox > .checkbox-label {
  font-size: inherit;
  font-family: inherit;
  line-height: inherit;
  display: inline-block;
  margin-bottom: 0;
  padding: 8px 15px 5px;
  cursor: pointer;
  touch-action: manipulation;
}
.custom-checkbox > label::before {
  content: "";
  box-sizing: border-box;
  position: absolute;
  top: 0;
  left: 0;
  width: 25px;
  height: 25px;
  border: 2px solid currentColor;
  background: transparent;
}
.custom-checkbox > input[type="checkbox"]:focus + label::before {
  border-width: 4px;
  outline: 3px dashed #228bec;
}
.custom-checkbox > label::after {
  box-sizing: content-box;
  content: "";
  position: absolute;
  top: 11px;
  left: 9px;
  width: 18px;
  height: 7px;
  transform: rotate(-45deg);
  border: solid;
  border-width: 0 0 5px 5px;
  border-top-color: transparent;
  opacity: 0;
  background: transparent;
}
.custom-checkbox > input[type="checkbox"]:checked + label::after {
  opacity: 1;
}
@media only screen and (min-width: 40rem) {
  label,
  input,
  .custom-checkbox {
    font-size: 19px;
    font-size: 1.9rem;
    line-height: 1.31579;
  }
}
</style> 