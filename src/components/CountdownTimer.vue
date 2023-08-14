<template>
    <div class="container">
        <form>
            <div class="form-row">
                <div class="col">
                    <label>Year</label>
                    <input v-model="yearUser" type="number" class="form-control" placeholder="Enter Year">
                </div>
                
                <div class="col">
                    <label>Month</label>
                    <input v-model="monthUser" type="number" class="form-control" placeholder="Enter Month">
                </div>

                <div class="col">
                    <label>Day</label>
                    <input v-model="dateUser" type="number" class="form-control" placeholder="Enter Day">
                </div>
            </div>

            <div class="form-row">
                <div class="col">
                    <label>Hour</label>
                    <input v-model="hourUser" type="number" class="form-control" placeholder="Enter Hour">
                </div>

                <div class="col">
                    <label>Minute</label>
                    <input v-model="minuteUser" type="number" class="form-control" placeholder="Enter Minute">
                </div>

                <div class="col">
                    <label>Second</label>
                    <input v-model="secondUser" type="number" class="form-control" placeholder="Enter Second">
                </div>
            </div>
            
            <div class="pt-2">
                <button class="btn btn-primary" @click="setCountdownTarget">Start Countdown</button>
                <button class="btn btn-danger" @click="resetCountdown">Reset</button>
            </div>
        </form>
    </div>
    <div>

        
        <div class="container">
            <div v-if="timerStarted">
                <h5 v-if="!expired" class="Green">Timer Is On</h5>
                <h5 v-if="expired" class="Red">Timer Has Expired</h5>
            </div>
            

            <div class="countdown-item">{{ displayDays }} days</div>
            <div class="countdown-item">{{ displayHours }} hours</div>
            <div class="countdown-item">{{ displayMinutes }} minutes</div>
            <div class="countdown-item">{{ displaySeconds }} seconds</div>

            <div class="progress">
                <div class="progress-bar" role="progressbar" :style="{ width: progressBarWidth }"></div>
            </div>
        </div>
</div>
</template>

<script>
export default {
    props: ['year', 'month', 'date', 'hour', 'minute', 'second', 'millisecond'],
    data: ()=>({
        yearUser: 0,
        monthUser: 0,
        dateUser: 0,
        hourUser: 0,
        minuteUser: 0,
        secondUser: 0,
        displayDays:0,
        displayHours:0,
        displayMinutes:0,
        displaySeconds:0,
        loaded: false,
        expired: false,
        timerStarted :true
    }),
    computed:{
        _seconds:() => 1000,
        _minutes(){
            return this._seconds * 60
        },
        _hours(){
            return this._minutes * 60
        },
        _days(){
            return this._hours * 24
        },
        end() {
            return new Date(
                this.yearUser,
                this.monthUser - 1, // Months are 0-indexed in JavaScript
                this.dateUser,
                this.hourUser,
                this.minuteUser,
                this.secondUser,
                0
            );
        },
        progressBarWidth() {
            const totalTime = this.end - new Date();
            const remainingTime = totalTime - (this._days * this.displayDays + this._hours * this.displayHours + this._minutes * this.displayMinutes + this._seconds * this.displaySeconds);
            const percentage = (remainingTime / totalTime) * 100;
            return `${percentage}%`;
        }
    },
    mounted(){
        // Always start the countdown if timerStarted is true
        
    },
    methods:{
        setCountdownTarget(event){
            event.preventDefault();
            this.timerStarted = true;
            this.showRemaining();
        },
        resetCountdown() {
            this.loaded = false;
            this.expired = false;
            this.displayDays = 0;
            this.displayHours = 0;
            this.displayMinutes = 0;
            this.displaySeconds = 0;
            clearInterval(this.timer); // Clear the interval timer
        },
        formatNum(num){
            return num < 10 ? "0" + num : num;
        },
        showRemaining() {
            const timer = setInterval(() =>{
                const now = new Date()
                //const end = new Date(2024, 4, 22, 10, 10, 10, 10);
                const distance = this.end.getTime() - now.getTime();

                if(distance < 0){
                    clearInterval(timer);
                    this.expired = true;
                    return
                }

                const days = Math.floor((distance / this._days));
                const hours = Math.floor((distance % this._days) / this._hours);
                const minutes = Math.floor((distance % this._hours) / this._minutes);
                const seconds = Math.floor((distance % this._minutes) / this._seconds);

                this.displayMinutes = this.formatNum(minutes);
                this.displaySeconds = this.formatNum(seconds);
                this.displayHours = this.formatNum(hours);
                this.displayDays = this.formatNum(days);
                this.loaded = true;
            }, 1000);
        }
    }
}
</script>

<style scoped lang="scss">
body {
  font-family: 'Roboto', Arial, sans-serif;
  background-color: #f7f7f7;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  text-align: center;
  padding: 30px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.countdown-item {
  display: inline-block;
  margin: 0 20px;
  font-size: 32px;
  font-weight: bold;
  color: #333333;
}

.btn-primary {
  background-color: #3498db;
  border-color: #3498db;
  padding: 10px 20px;
  font-size: 16px;
  font-weight: bold;
  color: #ffffff;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s, border-color 0.3s;
}

.btn-danger {
  background-color: #e74c3c;
  border-color: #e74c3c;
  margin-left: 10px;
}

.btn-primary:hover,
.btn-danger:hover {
  background-color: #2c81ba;
  border-color: #2c81ba;
}
.progress {
    height: 20px;
    margin-top: 10px;
}

.progress-bar {
    transition: width 1s linear; /* Add smooth transition animation */
}
</style>