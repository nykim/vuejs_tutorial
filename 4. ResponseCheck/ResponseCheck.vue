<template>
    <div>
        <div id="screen" :class="state" @click="onClickScreen">{{message}}</div>
        <div>
            <div>Average Reaction Time: {{result.reduce((a,c)=> a+c, 0)/result.length || 0}}ms</div>
            <button @click="onReset">Reset</button>
        </div>
    </div>
</template>
<script>
    let startTime = 0;
    let endTime = 0;
    let timeout = null;
    export default {
        data() {
            return {
                result: [],
                state: 'waiting',
                message: 'Click to start',
            }
        },
        methods: {
            onReset() {
                this.result = [];
                // this.state = 'waiting';
                // this.message = 'Click to start';
            },

            onClickScreen() {
                if (this.state === 'waiting') {
                    this.state = 'ready';
                    this.message = 'Click when the color changes to Green';
                    timeout = setTimeout(()=> {
                        this.state = 'now';
                        this.message = 'Click NOW!!!';
                        startTime = new Date();
                    }, Math.floor(Math.random()*1000) + 2000);
                } else if (this.state === 'ready') {
                    clearTimeout(timeout);
                    this.state = 'waiting';
                    this.message = 'You clicked too early.  Click after the color changes to green.';
                } else if (this.state === 'now') {
                    endTime = new Date();
                    this.state = 'waiting';
                    this.message = 'Click to start';
                    this.result.push(endTime - startTime);
                }
            }
        },
    };
</script>
<style scoped>
  #screen {
     width: 300px;
     height: 200px;
     text-align: center;
     user-select: none;
   }
  #screen.waiting {
    background-color: aqua;
  }
  #screen.ready {
    background-color: red;
    color: white;
  }
  #screen.now {
    background-color: greenyellow;
  }
</style>