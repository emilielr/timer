<template>
  <div id="app">
    <BaseTimer :timeLeft="formattedTimeLeft"/>
    <!--<Button buttonText="Sets"/>
    <Button buttonText="Action"/>
    <Button buttonText="Break"/>-->
    <Button buttonText="Start"/>
    <button @click="startTimer">Start</button>
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
    <p>Du har gjennomførst {{finishedSets}} antall sets</p>
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
      sets: 0,
      action: 0,
      pause: 0,
      finishedSets: 0,
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
    },
    countdownTimer() {
      if (this.timerRunning == true) {
        this.timePassed++;
        console.log(this.timePassed)
        if (this.timePassed > this.timeLimit) {
          clearInterval(this.interval);

          // Code for interpreting with break inbetween sets
          // If the number is even, we know it should not be a pause

          // TODO: pause nr 1 har samme tid som action. etter det går det greit
          if (this.finishedSets % 2 == 0) {
            this.timeLimit = this.action;
            console.log("action")
          } else {
            this.timeLimit = this.pause;
            console.log("break")
          }
          // Code below for doing repeating countdowns depending on numbers of sets
          this.finishedSets++;
          console.log("sets til nå: " + this.finishedSets)
          console.log("dette er time limit nå: " + this.timeLimit)
          if (this.finishedSets < this.sets) {
            this.timerRunning = false;
            this.timePassed = 0;
            this.startTimer();
          }
        }
      }
    },
    setTimer() {
      var minutes = document.getElementById("setMinutes").value;
      var seconds = document.getElementById("setSeconds").value;
      this.timeLimit = +minutes*60 + +seconds;
    },
    calculate() {
      var sets = parseInt(document.getElementById("setSets").value);
      var action = parseInt(document.getElementById("setAction").value);
      var pause = parseInt(document.getElementById("setBreak").value);

      if (pause > 0) {
        this.sets = sets * 2 - 1;
      } else {
        this.sets = sets;
      }
      
      this.timeLimit = action;
      this.pause = pause;
      this.action = action;
      this.finishedSets = 1;

      console.log("Antall sets med action+pause: " + this.sets)
      console.log("Action: " + this.action)
      console.log("Break: " + this.pause);
      
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
