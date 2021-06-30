<template>
  <main>
      <h1 id="main-title">Todo List App</h1>
      <p id="sub-title">made with ❤️ by gaurav @myselfgroot</p>

      <div id="todo-container">
          <input 
            type="text" 
            class="input" 
            placeholder="What you want to add?"
            v-model="textField"
            @keyup.enter="addTodo">
          <div class="todo-list">
              <div class="todo-list-item row" v-for="todo, index in getTodos" :key="index">
                  <input type="checkbox" 
                    :checked="todo.isCompleted"
                    true-value="true"
                    false-value="false"
                    class="col-1 todo-list-item-checkbox"
                    @click="todo.isCompleted = !todo.isCompleted">
                  <p class="todo-list-item-text col-11">{{ todo.text }}</p>
              </div>
              <p style="padding: 1rem; margin: 2rem 0; text-align: center; font-size: 1.4rem;" v-if="!getTodos.length">Nothing Left Here</p>
          </div>
          <footer id="todo-footer" class="row">
              <p class="todos-info col-3">{{ notCompletedTodos }}</p>
              <div class="todo-footer-actions row col-4">
                  <div class="todo-footer-action col-1" @click="todosType='all'" :class="{ 'active': todosType === 'all'}">All</div>
                  <div class="todo-footer-action col-1" @click="todosType='active'" :class="{ 'active': todosType === 'active'}">Active</div>
                  <div class="todo-footer-action col-1" @click="todosType='completed'" :class="{ 'active': todosType === 'completed'}">Completed</div>
              </div>
              <div 
                style="cursor: pointer; user-select: none;" 
                class="todo-footer-clear col-3"
                @click="removeCompleted">Clear Completed</div>
          </footer>
      </div>
  </main>
</template>

<script>

import { ref, computed} from 'vue';

export default {
  name: 'Todo App',
  setup() {
    const textField = ref('');
    const todosType = ref('all');

    const todos = ref([
      {
        isCompleted: false,
        text: 'Eat Something'
      }
    ]);

    const notCompletedTodos = computed({
      get() {
        const nonCompletedCount = todos.value.filter(todo => !todo.isCompleted).length;
        if(nonCompletedCount == 0) {
          return 'All Done 👍';
        } else if(nonCompletedCount == 1) {
          return 'Only 1 Left 😊';
        } else {
          return `${nonCompletedCount} left 😇`;
        }
        
      }
    });

    const getTodos = computed({
      get() {
        const todosBasedOnType = todos.value.filter(todo => {
          if(todosType.value === 'all') return true;
          else if(todosType.value === 'active') return !todo.isCompleted;
          return todo.isCompleted;
        })

        return todosBasedOnType;
      }
    })
    const addTodo = () => {
      todos.value.push({
        isCompleted: false,
        text: textField.value
      })

      textField.value = '';
    }

    const removeCompleted = () => {
      todos.value.forEach((todo, index) => {
        if(todo.isCompleted) todos.value.splice(index, 1);
      })
    }

    return {
      textField,
      todos,
      notCompletedTodos,
      addTodo,
      removeCompleted,
      todosType,
      getTodos,
    }
  }
}
</script>
