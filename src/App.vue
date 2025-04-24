<script setup>
import {ref, watch, onMounted} from 'vue';
const title = ref('Todo list');
const placeholderString = ref('Add some task');
const inputValue = ref('');
const notes = ref([]);


// Load data on mount of component
onMounted(() => {
  const savedNotes = localStorage.getItem('todo-notes');
  if (savedNotes) {
    notes.value = JSON.parse(savedNotes);
  }
});

//Save in localStorage
const saveToLocalStorage = () => {
  localStorage.setItem('todo-notes', JSON.stringify(notes.value))
};

//Save on changes
watch(notes,() => {
  saveToLocalStorage();
}, {deep: true});

const addNote =() => {
  const value = String(inputValue.value);
  if (value.trim()) {
    notes.value.push({
      text: value.trim(),
      completed: false,
      id: Date.now()
    })
    inputValue.value = ''
  }
};

const removeNote = (id) => {
  // notes.value.splice(idx, 1)
  notes.value = notes.value.filter(note => note.id !== id)
};

const removeAllNotes = () => {
  notes.value = []
};

const toggleCompleted = (id) => {
  const note = notes.value.find(n => n.id === id);
  if (note) {
    note.completed = !note.completed;
  }
};

const toUppercase = (item) => {
  return item.text.toUpperCase();
};

watch(inputValue, (newValue) => {
  if (newValue?.length > 20) {
    inputValue.value = ''
  }
});
</script>

<template>
  <div class="container">
    <div class="card">
      <h1>{{ title }}</h1>
      <div class="form-control">
        <input type="text" name="" id=""
               :placeholder="placeholderString"
               v-model="inputValue"
               @keyup.enter="addNote"
        />
        <button type="button"
                class="btn primary"
                @click="addNote">Add</button>
      </div>
      <ul class="list"
          v-if="notes.length !== 0">
        <li class="list-item"
            v-for="(note, idx) in notes"
            :key="note.id">
          <span :class="{completed: note.completed}">
            {{idx + 1}}) {{toUppercase(note)}}
          </span>
          <div class="button-group">
            <button type="button"
                    class="btn primary"
                    @click="toggleCompleted(note.id)">{{ note.completed ? 'Undo' : 'Done' }}</button>
            <button type="button"
                    :class="['btn', 'danger']"
                    @click="removeNote(note.id)">Delete</button>
          </div>
        </li>
      </ul>
      <div class="card-statistic" v-if="notes.length !== 0">
        <strong>Total: {{notes.length}}</strong>
        <button type="button"
                class="btn danger"
                @click="removeAllNotes">Delete all</button>
      </div>
      <div v-else>No Notes! Add one.</div>
    </div>
  </div>

  <!--SOME VARIANTS HOW TO USE CLASSES-->
  <!--          <span :class="item.length > 5 ? 'primary' : 'bold'">({{idx + 1}}) {{toUppercase(item)}}</span>-->
  <!--          <span :class="{-->
  <!--            'primary': true,-->
  <!--            'bold': item.length > 5-->
  <!--          }">({{idx + 1}}) {{toUppercase(item)}}</span>-->
  <!--          <span :class="['bold', {'primary': item.length > 5}]">({{idx + 1}}) {{toUppercase(item)}}</span>-->

</template>

<style scoped>
  .container {
    background-color: #f0f2f5;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    padding: 40px 20px;
    align-items: center;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-rendering: optimizeLegibility;
  }

  .card {
    background-color: #fff;
    border-radius: 12px;
    padding: 24px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 600px;
  }

  .card:not(:last-child) {
    margin-bottom: 10px;
  }

  .form-control {
    display: flex;
    gap: 12px;
    margin-bottom: 20px;
  }

  h1 {
    font-size: 24px;
    margin-bottom: 20px;
    font-weight: 500;
    color: #1c1e21;
  }

  .btn {
    padding: 8px 14px;
    font-size: 14px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: background-color 0.2s;
    font-weight: 500;
  }

  .completed {
    text-decoration: line-through;
    color: #65676b;
  }

  .btn:hover {
    cursor: pointer;
  }

  .btn.primary {
    background-color: #1877f2;
    color: #fff;
  }

  .btn.primary:hover {
    background-color: #166fe5;
  }

  .btn.danger {
    background-color: #e4e6eb;
    color: #050505;
  }

  .btn.danger:hover {
    background-color: #d8dadf;
  }

  .button-group {
    display: flex;
    gap: 8px;

    .btn {
      font-size: 14px;
    }
  }

  input {
    flex: 1;
    font-size: 16px;
    border: 1px solid #ccd0d5;
    border-radius: 8px;
    padding: 10px 12px;
    background-color: #f5f6f7;
    transition: border-color 0.3s ease;

    :focus {
      outline: none;
      border-color: #1877f2;
      background-color: #fff;
    }
  }

  .list {
    padding: 0;
    margin: 0;
    list-style: none;
  }

  .list-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 0;
    border-bottom: 1px solid #dddfe2;
    gap: 1rem;
    min-width: 0;

    span {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      min-width: 0;
    }
  }

  .list-item:last-child {
    border-bottom: none;
  }
  
  .card-statistic {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
    font-weight: 500;
    color: #1c1e21;
  }

</style>
