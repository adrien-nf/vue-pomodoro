<template>
  <section :class="'background slow-transition ' + (type === POMODORO ? 'background-red' : 'background-green')">
    <section>
      <Navbar />
    </section>
    <section class="container">
      <div class="columns">
        <div class="column is-half is-offset-one-quarter pt-5">
          <PomodoroClock :pomodorosCompleted="pomodorosCompleted" :type="type" :tasks="tasks" @create="createTask" @toggleTask="toggleTask" @skip="skip"/>
        </div>
      </div>
    </section>
  </section>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import PomodoroClock from "./components/PomodoroClock.vue";
import { POMODORO, BIG_PAUSE, TINY_PAUSE } from "./consts/types";

export default {
  name: 'App',
  components: {
    Navbar,
    PomodoroClock,
  },
  data: () => ({
    POMODORO,
    type: POMODORO,
    tasks: [],
    pomodorosCompleted: 0,
    timerStartDate: null
  }),
  methods: {
    createTask(name) {
      this.tasks.push({
        name: name.substr(0, 50),
        id: this.tasks.length,
        done: false
      })
    },
    toggleTask(index) {
      let taskIndex = this.tasks.findIndex(e => e.id === index);
      this.tasks[taskIndex].done = !this.tasks[taskIndex].done;
    },
    skip() {
      this.nextStep();
    },
    nextStep() {
      switch(this.type) {
        case POMODORO:
          this.pomodorosCompleted = (this.pomodorosCompleted + 1) > 4 ? 0 : this.pomodorosCompleted + 1
          this.type = this.pomodorosCompleted === 4 ? BIG_PAUSE : TINY_PAUSE;
          break;
        case BIG_PAUSE:
          this.type = POMODORO;
          this.pomodorosCompleted = 0
          break;
        case TINY_PAUSE:
          this.type = POMODORO;
          break;
      }
    }
  }
}
</script>

<style scoped>
  .background {
    width: 100%;
    height: 100vh;
  }

  .background-red {
    background-color: #ff7661;
  }

  .background-green {
    background-color: #61ff83;
  }

  .is-tomato {
    background-color: #ff8f7d;
  }

  .is-pepperbell {
    background-color: #97ff7d;
  }

  .slow-transition {
    transition: 0.2s;
  }
</style>