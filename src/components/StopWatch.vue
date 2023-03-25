<template>
  <div class="stopwatches">
    <div v-for="(stopwatch, index) in stopwatches" :key="index" class="stopwatches__wrapper">
      <div v-bind:class="{'stopwatches_active': stopwatches[index].isRunning}" class="stopwatches__time">{{ formatTime(stopwatch.time) }}</div>
      <hr v-bind:class="{'stopwatches__lines_active': stopwatches[index].isRunning}" class="stopwatches__lines" />
      <div class="stopwatches__controls">
        <svg v-if="stopwatches[index].isRunning === false" class="stopwatches__control-btn" @click="startStopwatch(index)" width="17" height="20" viewBox="0 0 17 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M0 20V0L17 10L0 20Z" fill="#9E9E9E"/>
        </svg>
        <svg v-if="stopwatches[index].isRunning === true" class="stopwatches__control-btn" @click="pauseStopwatch(index)" width="10" height="20" viewBox="0 0 10 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect v-bind:class="{'stopwatches_active': stopwatches[index].isRunning}" x="7" width="3" height="20" fill="#9E9E9E"/>
          <rect v-bind:class="{'stopwatches_active': stopwatches[index].isRunning}" width="3" height="20" fill="#9E9E9E"/>
        </svg>
        <svg class="stopwatches__control-btn"  @click="resetStopwatch(index)" width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect v-bind:class="{'stopwatches_active': stopwatches[index].isRunning}" width="20" height="20" fill="#9E9E9E"/>
        </svg>        
      </div>      
    </div>
    <div class="stopwatches__add" @click="addStopwatch()">
      <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
        <rect x="8.5" width="3" height="20" fill="#9E9E9E"/>
        <rect y="11.5" width="3" height="20" transform="rotate(-90 0 11.5)" fill="#9E9E9E"/>
      </svg>
    </div>
  </div>
</template>

<style>

.stopwatches {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 45px 50px;
  max-width: 775px;
  margin: 0 auto
}

.stopwatches__wrapper {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  background: #696969;
  width: 225px;
  height: 120px;
}

.stopwatches__time {
  font-family: 'Gotham Pro';
  font-style: normal;
  font-weight: 400;
  font-size: 22px;
  line-height: 21px;
  text-align: center;
  color: #9E9E9E;
}

.stopwatches__lines {
  border: 1px solid #9E9E9E;
  width: 100%;
  box-sizing: border-box;
}

.stopwatches__lines_active {
  border: 1px solid #fff;
}

.stopwatches__controls {
  display: flex;
}

.stopwatches__control-btn {  
  cursor: pointer;
  margin-right: 48px;
  width: 20px;
  height: 20px;
}

.stopwatches__control-btn:last-child {
  margin-right: 0;
}

.stopwatches_active {
  fill: #fff;
  color: #fff;
}

.stopwatches__add {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 225px;
  height: 120px;
  background: #696969;
  cursor: pointer;
}

@media (max-width: 768px) {
  .stopwatches {
  grid-template-columns: repeat(2, 1fr);
  max-width: 500px;
}
}

@media (max-width: 320px) {
  .stopwatches {
    grid-template-columns: 1fr;
    max-width: 225px;
  }
}

</style>

<script>
export default {
  data() {
    return {
      stopwatches: [
        {
          time: 0,
          intervalId: null,
          lastTick: 0,
          isRunning: false,
        },
        {
          time: 0,
          intervalId: null,
          lastTick: 0,
          isRunning: false,
        },
        {
          time: 0,
          intervalId: null,
          lastTick: 0,
          isRunning: false,
        },
        {
          time: 0,
          intervalId: null,
          lastTick: 0,
          isRunning: false,
        },
        {
          time: 0,
          intervalId: null,
          lastTick: 0,
          isRunning: false,
        },
      ],
    };
  },
  methods: {
    startStopwatch(stopwatchIndex) {
      const stopwatch = this.stopwatches[stopwatchIndex];
      if (!stopwatch.isRunning) {
        stopwatch.isRunning = true;
        stopwatch.lastTick = Date.now();
        stopwatch.intervalId = setInterval(() => {
          const now = Date.now();
          const elapsed = now - stopwatch.lastTick;
          stopwatch.time += elapsed;
          stopwatch.lastTick = now;
        }, 10);
      }
    },
    pauseStopwatch(stopwatchIndex) {
      const stopwatch = this.stopwatches[stopwatchIndex];
      if (stopwatch.isRunning) {
        stopwatch.isRunning = false;
        clearInterval(stopwatch.intervalId);
      }
    },
    resetStopwatch(stopwatchIndex) {
      const stopwatch = this.stopwatches[stopwatchIndex];
      stopwatch.time = 0;
      stopwatch.lastTick = 0;
      stopwatch.isRunning = false;
      clearInterval(stopwatch.intervalId);
    },
    addStopwatch() {
      this.stopwatches.push({
        time: 0,
        intervalId: null,
        lastTick: 0,
        isRunning: false,
      });
    },
    formatTime(timeInMs) {
      const hours = Math.floor(timeInMs / 3600000);
      const minutes = Math.floor((timeInMs - hours * 3600000) / 60000);
      const seconds = Math.floor((timeInMs - hours * 3600000 - minutes * 60000) / 1000);
      if (minutes <= 0 && hours <= 0) {
        return `${String(seconds).padStart(2, '0')}`;
      } else if (minutes >= 0 && hours <= 0) {
        return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
      } else if (minutes >= 0 && hours >= 0) {
        return `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
      }      
    },
  },
};
</script>