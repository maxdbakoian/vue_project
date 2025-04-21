<script>
export default {
  data() {
    return {
      title: 'Todo list',
      placeholderString: 'Add some task',
      inputValue: '',
      notes: []
    }
  },
  methods: {
    //changed, while I use v-model="inputValue"

    // inputChangeHandler(event) {
    //   this.inputValue = event.target.value;
    // },
    addNote() {
      if (this.inputValue !== '') {
        this.notes.push(this.inputValue)
        this.inputValue = ''
      }
    },
    removeNote(idx) {
      this.notes.splice(idx, 1)
    },
    removeAllNotes() {
      this.notes = []
    },
    toUppercase(item) {
      return item.toUpperCase();
    }
  },
  computed: {
    doubleCountComputed() {
      return this.notes.length * 2
    }
  },
  watch: {
    inputValue(value) {
      if (value.length > 10) {
        this.inputValue = ''
      }
    }
  }
}

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
        <button type="button" class="btn primary" @click="addNote">Add</button>
        <hr/>
      </div>
      <ul class="list" v-if="notes.length !== 0">
        <li class="list-item" v-for="(item, idx) in notes">
<!--SOME VATIANTS HOW TO USE CLASSES-->
<!--          <span :class="item.length > 5 ? 'primary' : 'bold'">({{idx + 1}}) {{toUppercase(item)}}</span>-->
<!--          <span :class="{-->
<!--            'primary': true,-->
<!--            'bold': item.length > 5-->
<!--          }">({{idx + 1}}) {{toUppercase(item)}}</span>-->
<!--          <span :class="['bold', {'primary': item.length > 5}]">({{idx + 1}}) {{toUppercase(item)}}</span>-->
          {{idx + 1}}) {{toUppercase(item)}}
          <button type="button" class="btn danger" @click="removeNote(idx, $event)">Delete</button>
        </li>
      </ul>
      <div class="card-statistic" v-if="notes.length !== 0">
        <strong>Common quantity: {{notes.length}} | Doubled: {{doubleCountComputed}}</strong>
        <button type="button" class="btn danger" @click="removeAllNotes()">Delete all</button>
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
    flex-direction: column;
  }

  h3 {
    font-weight: lighter;
  }

  .btn {
    align-items: center;
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
    background-color: darkseagreen;
  }

  .btn.danger:hover {
    background-color: #75bd75;
  }

  input {
    font-size: 18px;
    border: 4px solid #5280FF;
    border-radius: 8px;
    padding: 10px;
    margin-bottom: 10px;
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
