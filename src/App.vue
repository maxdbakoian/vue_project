<script setup>
import {ref, computed, watch} from 'vue';
const title = ref('Todo list');
const placeholderString = ref('Add some task');
const inputValue = ref('');
const notes = ref([]);


const addNote =() => {
  const value = String(inputValue.value);
  if (value.trim()) {
    notes.value.push({
      text: value.trim(),
      completed: false
    })
    inputValue.value = ''
  }
};

const removeNote = (idx) => {
  notes.value.splice(idx, 1)
};

const removeAllNotes = () => {
  notes.value = []
};

const toggleCompleted = (idx) => {
  notes.value[idx].completed = !notes.value[idx].completed;
}

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
            v-for="(item, idx) in notes"
            :key="idx">
          <span :class="{completed: item.completed}">
            {{idx + 1}}) {{toUppercase(item)}}
          </span>
          <div class="button-group">
            <button type="button"
                    class="btn primary"
                    @click="toggleCompleted(idx)">{{ item.completed ? 'Undo' : 'Done' }}</button>
            <button type="button"
                    :class="['btn', 'danger']"
                    @click="removeNote(idx, $event)">Delete</button>
          </div>

          <!--SOME VATIANTS HOW TO USE CLASSES-->
          <!--          <span :class="item.length > 5 ? 'primary' : 'bold'">({{idx + 1}}) {{toUppercase(item)}}</span>-->
          <!--          <span :class="{-->
          <!--            'primary': true,-->
          <!--            'bold': item.length > 5-->
          <!--          }">({{idx + 1}}) {{toUppercase(item)}}</span>-->
          <!--          <span :class="['bold', {'primary': item.length > 5}]">({{idx + 1}}) {{toUppercase(item)}}</span>-->

        </li>
      </ul>
      <div class="card-statistic" v-if="notes.length !== 0">
        <strong>Common quantity: {{notes.length}}</strong>
        <button type="button"
                class="btn danger"
                @click="removeAllNotes">Delete all</button>
      </div>
      <div v-else>No Notes! Add one.</div>
    </div>
  </div>
</template>

<style scoped>
  .container {
    background-color: #2c3e50;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    padding: 20px;
  }

  .card {
    background-color: #fff;
    border-radius: 10px;
    padding: 20px;
  }

  .card:not(:last-child) {
    margin-bottom: 10px;
  }

  .form-control {
    display: flex;
    justify-content: space-between;
    column-gap: 16px;
  }

  h3 {
    font-weight: lighter;
  }

  .btn {
    align-items: center;
    color: #000;
    appearance: none;
    border: none;
    border-radius:8px;
    display: flex;
    align-self: flex-start;
    font-size: 18px;
    line-height: 1.15;
    padding: 13px 16px;
    text-align: center;
    transition: background-color .3s, color .3s, box-shadow .3s;
    justify-content: center;
    text-decoration: none;
  }

  .completed {
    text-decoration: line-through;
    opacity: 0.7;
    background-color: #f0f0f0;
  }

  .btn:hover {
    cursor: pointer;
  }

  .btn.primary {
    background-color: #5280FF;
  }

  .btn.primary:hover {
    background-color: #265ef9;
  }

  .btn.danger {
    background-color: #ff2c2c;
  }

  .btn.danger:hover {
    background-color: #e52525;
  }

  .button-group {
    display: flex;
    column-gap: 16px;

    .btn {
      font-size: 14px;
    }
  }

  input {
    font-size: 18px;
    border: 4px solid #5280FF;
    border-radius: 8px;
    padding: 10px;
    margin-bottom: 10px;
    width: 100%;
  }

  .list {
    padding-left: 0;
    list-style: unset;
    appearance: none;
  }

  .list-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 0;
  }

  .list-item:not(:first-child) {
    border-top: 1px solid black;
  }
  
  .card-statistic {
    align-items: center;
    display: flex;
    justify-content: space-between;
  }

</style>
