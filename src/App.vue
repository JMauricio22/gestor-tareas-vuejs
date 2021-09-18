<template>
  <div class="container">
    <div class="my-3">
      <h1 class="title">GESTOR DE TAREAS</h1>
    </div>
    <div class="section">
      <h2 class="title">Nueva Tarea</h2>
      <div class="box">
        <form @submit.prevent="addTask">
          <div class="block">
            <input
              type="text"
              v-model="newTask.title"
              class="input is-medium is-info"
              placeholder="Nombre de la tarea"
            />
          </div>
          <div class="block">
            <input
              type="number"
              v-model="newTask.time"
              class="input is-medium is-info"
              placeholder="Horas"
            />
          </div>
          <div class="columns">
            <div class="column is-12">
              <div class="is-flex">
                <div class="mr-2">
                  <button class="button is-info" type="submit">
                    Agregar
                  </button>
                </div>
                <div class="mr-2">
                  <button class="button is-danger" type="button" @click="resetForm">
                    Cancelar
                  </button>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
    <div class="section">
      <div class="box">
        <h2 class="title">Lista de tareas</h2>
        <div v-show="tasks.length > 0" class="mb-2">
          <span class="tag is-link is-medium">
            Total de horas trabajadas: {{totalTime}}.
          </span>
        </div>
        <div v-show="tasks.length === 0" class="notification is-danger">
          <p>Todavia no tiene ninguna tarea 😢</p>
          <p>Presione agregar para crear una nueva tarea.</p>
        </div>
        <Task v-for="(task, index) in tasks"  v-bind="task" :key="task.title" @remove='removeTask(index)'>
        </Task>
      </div>
    </div>
  </div>
</template>

<script>
import Task from './components/Task.vue'

export default {
  name: 'App',
  created () {
    const tasks = localStorage.getItem('tasks')
    if (tasks) {
      this.tasks = JSON.parse(tasks)
    }
  },
  data () {
    return {
      newTask: {
        title: '',
        time: 0
      },
      tasks: []
    }
  },
  methods: {
    addTask () {
      if (!this.newTask.title || !this.newTask.time) {
        return
      }
      this.tasks.push({
        ...this.newTask
      })
      this.resetForm()
      this.updateLocalStorage()
    },
    removeTask (index) {
      this.tasks.splice(index, 1)
      this.updateLocalStorage()
    },
    updateLocalStorage () {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    resetForm () {
      this.newTask.title = ''
      this.newTask.time = 0
    }
  },
  computed: {
    totalTime () {
      return this.tasks.reduce((acc, item) => {
        return typeof acc === 'number' ? acc + item.time : acc.time + item.time
      }, 0)
    }
  },
  components: {
    Task
  }
}
</script>

<style lang="scss">
@import "../node_modules/bulma/bulma.sass";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
