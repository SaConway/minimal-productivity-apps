<template>
  <div class="container">
    <button v-if="data.status != 'off'" @click="stopTimer">Stop</button>

    <button v-if="data.status == 'on'" @click="pauseTimer" key="pause-btn">Pause</button>
    <button v-else-if="data.status == 'pause'" @click="resumeTimer">Resume</button>

    <template v-if="data.status == 'off'">
      <TimerInput />
      <button @click="startTimer" class="start-btn" key="start-btn">Start</button>
    </template>

    <template v-if="data.countdown != null">
      <p class="countdown">{{ data.countdown }}</p>

      <div class="progress">
        <div class="bar" :style="{ width: data.progress + '%' }"></div>
      </div>
    </template>
  </div>
</template>

<script>
import { timerStore } from "../store/timerStore";
import TimerInput from "../components/TimerInput";

export default {
  components: {
    TimerInput
  },
  data: function() {
    return {
      data: timerStore.state.output
    };
  },
  methods: {
    startTimer() {
      timerStore.startTimer();
    },
    stopTimer() {
      timerStore.stopTimer();
    },
    pauseTimer() {
      timerStore.pauseTimer();
    },
    resumeTimer() {
      timerStore.resumeTimer();
    }
  }
};
</script>

<style scoped>
.container {
  width: auto;
  text-align: center;
}

button {
  color: var(--clr-accent);
  width: 7rem;
  margin-right: 1rem;
  padding: 1rem;
  font-size: var(--font-size-normal);
  border: 1px solid var(--clr-accent);
  border-radius: var(--border-radius);
}

p.countdown {
  color: var(--clr-accent);
  font-size: 4rem;
  letter-spacing: 0.5rem;
  text-align: center;
  margin-top: 8rem;
  animation: fade-in 0.5s ease-in;
}

.progress {
  height: 15px;
  width: 30rem;
  margin: 2rem auto;
  border-radius: var(--border-radius);
  background-color: #eee;
  border: 1px solid #222831;
  overflow: hidden;
  animation: fade-in 0.5s ease-in;
}

.bar {
  height: 15px;
  background-color: var(--clr-secondary);
  transition: width 0.1s linear;
}

@media (max-width: 1000px) {
  p.countdown {
    font-size: 3rem;
  }
  .progress {
    width: 80%;
  }
  .start-btn {
    display: block;
    margin-top: 2rem;
  }
}
</style>
