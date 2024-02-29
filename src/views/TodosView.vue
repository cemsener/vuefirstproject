<template>
  <div>
    <div v-if="selectedQuote" class="quote-container">
      <p class="quote">{{ selectedQuote.quote }}</p>
      <p class="author">- {{ selectedQuote.author }}</p>
    </div>

    <!-- Todo App Başlangıcı -->
    <div>
      <h2>💪 Hemen planlamaya başla başla 💪</h2>
      <div class="todo-input-container">
        <input
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="Gelişeceğin adımı yaz ..."
          class="todo-input"
        />
      </div>
      <ul class="todo-list">
        <li v-for="(todo, index) in todos" :key="index" class="todo-item">
          {{ todo }}
          <span class="todo-delete" @click="deleteTodo(index)">❌</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      quotes: [], //alınan alıntılar bu diziye gidecek
      selectedQuote: null, //seçilen alıntı bu değişkene gidecek
      todos: [],
      newTodo: ''
    }
  },
  methods: {
    selectRandomQuote() {
      if (this.quotes.length > 0) {
        const randomIndex = Math.floor(Math.random() * this.quotes.length)
        this.selectedQuote = this.quotes[randomIndex]
      }
    },
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push(this.newTodo)
        this.newTodo = ''
      }
    },
    deleteTodo(index) {
      this.todos.splice(index, 1)
    }
  },
  created() {
    axios
      .get('https://dummyjson.com/quotes')
      .then((response) => {
        this.quotes = response.data.quotes
        this.selectRandomQuote()
        console.log('Seçilen alıntı', this.selectedQuote)
      })
      .catch((error) => {
        console.log('API Error', error)
      })
  }
}
</script>

<style>
.quote-container {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  text-align: center;
}

.quote {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
  color: white;
}

.author {
  font-size: 18px;
  font-style: italic;
}

.todo-app {
  margin-top: 50px;
}

.todo-title {
  text-align: center;
  color: #333;
  margin-top: 50px;
}

.todo-input-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
  margin-top: 20px;
}

.todo-input {
  width: 60%;
  padding: 10px;
  font-size: 16px;
  border: 2px solid #ccc;
  border-radius: 5px;
}

.todo-list {
  list-style: none;
  padding: 0;
  background-color: transparent;
}

.todo-item {
  margin: 10px 0;
  padding: 10px;
  border-radius: 5px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.todo-delete {
  cursor: pointer;
  color: red;
  font-weight: bold;
}
</style>
