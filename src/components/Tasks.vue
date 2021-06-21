<template>
  <div class="columns">
    <div class="column">
      <div class="columns">
        <div class="column is-9">
          <input v-model="taskName" class="input is-danger" type="text" placeholder="Task name" v-on:keydown.enter="createTask">
        </div>
        <div class="column is-3">
          <button class="button is-fullwidth" @click="createTask">Create</button>
        </div>
      </div>
      <div class="columns pt-2">
        <div class="column">
          <h1>On Going</h1>
          <nav class="panel">
            <a class="panel-block" style="display:block" v-for="(task) in onGoingTasks" :key="task.id" @click="() => toggleTask(task.id)">
              <span class="panel-icon has-text-light">
                <i class="fas fa-book" aria-hidden="true"></i>
              </span>
              {{task.name}}
              <svg width="15" height="15">       
                  <image xlink:href="tomato.svg" width="15" height="15"/>    
              </svg>
            </a>
          </nav>
          <h1>Done</h1>
          <nav class="panel">
            <a class="panel-block" style="display:block" v-for="(task) in doneTasks" :key="task.id" @click="() => toggleTask(task.id)">
              <span class="panel-icon has-text-light">
                <i class="fas fa-book" aria-hidden="true"></i>
              </span>
              {{task.name}}
              <svg width="15" height="15">       
                  <image xlink:href="bell-pepper.svg" width="15" height="15"/>    
              </svg>
            </a>
          </nav>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    taskName: ""
  }),
  props: {
    tasks: {}
  },
  methods: {
    createTask() {
      if(this.taskName.length > 0) {
        this.$emit("create", this.taskName);
        this.taskName = ""
      }
    },
    toggleTask(index) {
      this.$emit("toggleTask", index)
    }
  },
  computed: {
    doneTasks() {
      return this.tasks.filter(e => e.done)
    },
    onGoingTasks() {
      return this.tasks.filter(e => !e.done)
    }
  }
}
</script>

<style>

</style>