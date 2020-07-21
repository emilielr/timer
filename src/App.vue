<template>
  <div id="app">
    <BaseTimer :timeLeft="formattedTimeLeft"/>
    <!--<Button buttonText="Sets"/>
    <Button buttonText="Action"/>
    <Button buttonText="Break"/>-->
    <Button buttonText="Start"/>
    <button @click="calculate">Start</button>
    <button @click="pauseTimer">Pause</button>
    <button @click="resetTimer">Reset</button>
    <br><br>
    <label for="setSets">Sets: </label>
    <input id="setSets" type ="number">
    <label for="setAction">Action: </label>
    <input id="setAction" type ="number">
     <label for="setBreak">Break: </label>
    <input id="setBreak" type ="number">
    <input type="submit" value="calc set" @click="calculate">
    <p>Du har gjennomført {{finishedActionSets}} antall sets</p>
  </div>
</template>

<script>
import BaseTimer from './components/BaseTimer.vue'
import Button from './components/Button.vue'

export default {
  name: 'App',
  components: {
    BaseTimer,
    Button,
  },

  data() {
    return {
      timeLimit: 0,
      timePassed: 0,
      interval: null,
      timerRunning: false,
      totalSets: 0,
      sets: 0,
      action: 0,
      pause: 0,
      finishedSets: 0,
      finishedActionSets: 0,
    }
  },

  computed: {
    timeLeft() {
      return this.timeLimit - this.timePassed;
    },
    formattedTimeLeft() {
      const timeLeft = this.timeLeft;
      const minutes = Math.floor(timeLeft/60);
      let seconds = timeLeft % 60;
      if (seconds < 10) {
        seconds = `0${seconds}`
      }
      return `${minutes}:${seconds}`;
    },
  },

  methods: {
    startTimer() {
      if (!this.timerRunning) {
        this.timerRunning = true;
        this.interval = setInterval(this.countdownTimer, 1000)
      }
    },
    pauseTimer() {
      this.timerRunning = false;
      clearInterval(this.interval);
    },
    resetTimer() {
      this.timerRunning = false;
      clearInterval(this.interval);
      this.timePassed = 0;
      this.timeLimit = this.action;
      this.finishedSets = 0;
      this.totalSets = 0;
      this.pause = 0;
      this.action = 0;
      this.sets = 0;
      this.finishedActionSets = 0;
    },
    countdownTimer() {
      if (this.timerRunning == true) {
        this.timePassed++;
        console.log(this.timePassed)
        if (this.timePassed >= this.timeLimit) {
          clearInterval(this.interval);
          
          if (this.finishedSets == 0) {
            this.finishedSets++;
          }
         
          // Code for interpreting with break inbetween sets
          // If the number is even, we know it should not be a pause
          if (this.finishedSets % 2 == 0) {
            this.timeLimit = this.action;
            console.log("Next is action")
          } else {
            // TODO: I added this if-statement and now it stops at the correct time, but it must be a better way
            // Has to check if timer has breaks or not to set the timeLimit properly and get correct time
            if (this.finishedSets != this.totalSets) {
              if (this.pause != 0) {
                this.timeLimit = this.pause;
              }
            }
            this.finishedActionSets++;
            console.log("Next is break")
          }

          // Code for doing repeating countdowns depending on numbers of sets
          this.finishedSets++;
          console.log("Finished sets: " + this.finishedSets)
          console.log("Time limit: " + this.timeLimit)
          if (this.finishedSets <= this.totalSets) {
            this.timerRunning = false;
            this.timePassed = 0;
            this.startTimer();
          }
        }
      }
    },
    
    calculate() {
      var sets = parseInt(document.getElementById("setSets").value);
      var action = parseInt(document.getElementById("setAction").value);
      var pause = parseInt(document.getElementById("setBreak").value);

      if (pause > 0) {
        this.totalSets = sets * 2 - 1;
      } else if (isNaN(sets)) {
        this.totalSets = 0;
      } else {
        this.totalSets = sets;
      }

      if (isNaN(pause)) {
        this.pause = 0;
      } else {
        this.pause = pause;
      }
      
      this.timeLimit = action;
      this.action = action;

      console.log("Number of sets (action + breaks): " + this.totalSets)
      console.log("Action: " + this.action)
      console.log("Break: " + this.pause);
      console.log("Finished sets: " + this.finishedSets)
      
      this.startTimer();
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
