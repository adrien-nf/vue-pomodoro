<template>
  <section :class="'background slow-transition ' + (type === POMODORO ? 'background-red' : 'background-green')">
    <section>
      <Navbar />
    </section>
    <section class="container">
      <div class="columns">
        <div class="column is-half is-offset-one-quarter pt-5">
          <PomodoroClock
          :currentTimer="currentTimer"
          :elapsedTime="elapsedTime"
          :pomodorosCompleted="pomodorosCompleted"
          :type="type"
          :tasks="tasks"
          @create="createTask"
          @toggleTask="toggleTask"
          @skip="skip"
          @setStep="setStep"
          @toggleTimer="toggleTimer"
          :timer="timer"
          />
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
    elapsedTime: 0,
    timer: null,
    settings: {
      pomodoroTimer: 25 * 60,
      tinyPauseTimer: 5 * 60,
      bigPauseTimer: 15 * 60,
    },
    currentTimer: 25 * 60,
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
      this.elapsedTime = 0;
      switch(this.type) {
        case POMODORO:
          this.pomodorosCompleted = (this.pomodorosCompleted + 1) > 4 ? 0 : this.pomodorosCompleted + 1
          this.type = this.pomodorosCompleted === 4 ? BIG_PAUSE : TINY_PAUSE;
          this.stopTimer();
          this.currentTimer = this.type === BIG_PAUSE ? this.settings.bigPauseTimer : this.settings.tinyPauseTimer;
          this.startTimer();
          break;
        case BIG_PAUSE:
          this.type = POMODORO;
          this.pomodorosCompleted = 0
          this.stopTimer();
          this.currentTimer = this.settings.pomodoroTimer;
          this.startTimer();
          break;
        case TINY_PAUSE:
          this.type = POMODORO;
          this.stopTimer();
          this.currentTimer = this.settings.pomodoroTimer;
          this.startTimer();
          break;
      }
    },
    setStep(step) {
      this.pomodorosCompleted = 0;
      this.type = step;
      this.elapsedTime = 0;
      if(step === POMODORO) {
        this.currentTimer = this.settings.pomodoroTimer
      } else if (step === BIG_PAUSE) {
        this.currentTimer = this.settings.bigPauseTimer
      } else if (step === TINY_PAUSE) {
        this.currentTimer = this.settings.tinyPauseTimer
      }
    },
    startTimer() {
      this.timer = setInterval(() => {
        this.elapsedTime++;
        if(this.elapsedTime === this.currentTimer) {
          this.nextStep();
        }
      }, 1000)
    },
    stopTimer() {
      if(this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }
    },
    toggleTimer() {
      if(this.timer) {
        this.stopTimer();
      } else {
        this.startTimer();
      }
    }
  },
  mounted() {
    this.currentTimer = this.settings.pomodoroTimer
    this.startTimer();
  }
}
</script>

<style scoped>
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