<template>
  <div class="columns">
      <div class="column">
          <p>{{title}}</p>
          <h1 class="timer-time">25:00</h1>
          <p>
              {{pomodorosCompleted}}
          </p>
          <button class="button is-fullwidth" @click="$emit('skip')">Skip</button>
      </div>
  </div>
</template>

<script>
import { BIG_PAUSE, POMODORO, TINY_PAUSE } from '../consts/types'
export default {
    props: {
        type: {},
        pomodorosCompleted: {},
    },
    computed: {
        title() {
            let v = null;
            switch(this.type) {
                case POMODORO:
                    v = "Pomodoro"
                    break;
                case BIG_PAUSE:
                    v = "Big Pause"
                    break;
                case TINY_PAUSE:
                    v = "Short Pause"
                    break;
            }
            return v;
        }
    }
}
</script>

<style>
    .timer-time {
        font-size: 5em;
        text-align: center;
    }
</style>