<template>
  <div>
    <header class="navbar">
      <nav>
        <ul>
          <li @click="showTodos">Todos</li>
        </ul>
      </nav>
    </header>
    <div class="container">
      <h1 v-if="activeTab === 'todos'">CLUB BOLA</h1>
      <div v-if="activeTab === 'todos'">
        <input
          type="text"
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="Masukkan Nama Club"
        />
        <ul>
          <li v-for="(todo, index) in filteredTodos" :key="index">
            <input type="checkbox" v-model="todo.completed" />
            <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
            <button @click="removeTodo(index)">Hapus</button>
          </li>
        </ul>
        <p v-if="remainingCount === 0">Ayo Bertanding.</p>
        <p v-else>{{ remainingCount }} task(s) remaining</p>
        <button @click="toggleFilter">{{ filterButtonText }}</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      showCompleted: false,
      activeTab: "todos",
      users: [],
    };
  },
  computed: {
    filteredTodos() {
      return this.showCompleted
        ? this.todos
        : this.todos.filter((todo) => !todo.completed);
    },
    remainingCount() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    filterButtonText() {
      return this.showCompleted ? "Show Uncompleted" : "Show All";
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== "") {
        this.todos.push({ text: this.newTodo, completed: false });
        this.newTodo = "";
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleFilter() {
      this.showCompleted = !this.showCompleted;
    },
    async fetchUsers() {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users"
        );
        this.users = await response.json();
      } catch (error) {
        console.error("Error fetching users:", error);
      }
    },
    showTodos() {
      this.activeTab = "todos";
    },
  },
  mounted() {
    this.fetchUsers();
  },
};
</script>

<style>
.completed {
  text-decoration: line-through;
}

.navbar {
  background-color: rgb(26, 21, 86);
  overflow: hidden;
  display: flex; /* Tambahkan */
  justify-content: center; /* Tambahkan */
}

.navbar ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.navbar ul li {
  float: left;
}
</style>
