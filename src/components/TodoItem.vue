<script setup>
import { Icon } from '@iconify/vue';

const props = defineProps({
    todo: {
        type: Object,
        required: true,
    },
    index: {
        type: Number,
        required: true,
    }
});

defineEmits(['toggle-complete', "edit-todo", "update-todo", "delete-todo"]);
</script>

<template>
    <!-- we'll be building a list of todo's here -->
    <li>
        <!-- making checkbox button...
            when user clicks it (upon input), we emit a custom-made event 'toggle-complete'
            along with the toggle's index to the parent component (TodosView) -->
        <input 
            type="checkbox" 
            :checked="todo.isCompleted" 
            @input="$emit('toggle-complete', index)" />
        
        <!-- making text part of todo...
            - if it's in editing mode, change mode to text and allow user to edit
                and when done editing, emit a new event 'update-todo' that sends todo's current value ($event.target.value) and index
            - but otherwise just show plain text 
                and if completed, strike thru text (by simply adding class-specific CSS style) -->
        <div class="todo">
            <input  v-if="todo.isEditing" type="text" :value="todo.todo" 
                @input="$emit('update-todo', $event.target.value, index)" />
            <span v-else :class="{'completed-todo' : todo.isCompleted}">
                {{ todo.todo }}
            </span>
        </div>
        
        <!-- making action item buttons 
            - pencil icon: in editing mode
            - check-circle icon: finished editing the todo
            - trash icon: delete todo 
            ** we use the same @click="$emit('edit-todo', index)" for both pencil & check-circle
            because it just toggles the editing status of the todo (true <-> false) -->
        <div class="todo-actions">
            <Icon v-if="todo.isEditing" 
                icon="ph:check-circle" class="icon" 
                style="color: #379046" width="22" 
                @click="$emit('edit-todo', index)" />

            <Icon v-else icon="ph:pencil-fill" class="icon" 
                style="color: #379046" width="22" 
                @click="$emit('edit-todo', index)"/>

            <Icon icon="ph:trash" class="icon" 
                style="color: #f95e5e" width="22"
                @click="$emit('delete-todo', todo.id)" />
        </div>
    </li>
</template>

<style lang="scss" scoped>
li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16px 10px;
  background-color: #f1f1f1;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);

  &:hover {
    .todo-actions {
      opacity: 1;
    }
  }

  input[type="checkbox"] {
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);

    &:checked {
      background-color: #41b080;
    }
  }

  .todo {
    flex: 1;

    .completed-todo {
        text-decoration: line-through;
    }

    input[type="text"] {
      width: 100%;
      padding: 2px 6px;
      border: 2px solid #41b080;
    }
  }

  .todo-actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;
    .icon {
      cursor: pointer;
    }
  }
}
</style>