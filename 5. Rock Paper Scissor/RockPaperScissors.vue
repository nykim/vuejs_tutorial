<template>
    <div>
        <div id="computer" :style="computedStyleObject"></div>
        <div>
            <button @click="onClickButton('ROCK')">Rock</button>
            <button @click="onClickButton('PAPER')">Paper</button>
            <button @click="onClickButton('SCISSORS')">Scissors</button>
        </div>
        <div>{{result}}</div>
        <div>Current Score : {{score}}</div>
    </div>
</template>
<script>
    const respCoords = {
        ROCK: '0',
        SCISSORS: '-142px',
        PAPER: '-284px',
    };

    const scores = {
        SCISSORS: 1,
        ROCK: 0,
        PAPER: -1,
    };

    const computerChoice = (imgCoord) => {
        return Object.entries(respCoords).find(function(v) {
            return v[1] === imgCoord;
        })[0];
    }

    let interval = null;
    export default {
        data() {
            return {
                imgCoord: respCoords.PAPER,
                result: '',
                score: 0,
            };
        },
        computed: {
            computedStyleObject() {
                return { background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0` };
            }
        },
        methods: {
            changeHand() {
                interval = setInterval(() => {
                    if (this.imgCoord === respCoords.ROCK) {
                        this.imgCoord = respCoords.SCISSORS;
                    } else if (this.imgCoord === respCoords.SCISSORS) {
                        this.imgCoord = respCoords.PAPER;
                    } else if (this.imgCoord === respCoords.PAPER) {
                        this.imgCoord = respCoords.ROCK;
                    }
                }, 100);
            },

            onClickButton(choice) {
                clearInterval(interval);
                const myScore = scores[choice];
                const cpuScore = scores[computerChoice(this.imgCoord)];
                const diff = myScore - cpuScore; 
                if (diff===0) {
                    this.result = "Tie";
                } else if ([-1, 2].includes(diff)) {
                    this.result = "Win!";
                    this.score += 1;
                } else {
                    this.result = "Lose!";
                    this.score -= 1;
                }
                setTimeout(() => {
                    this.changeHand();
                }, 1000);
            },
        },

        beforeCreate() {
            console.log('beforeCreate');
        },
        created() {
            console.log('created');
        },
        beforeMount() {
            console.log('beforeMount');
        },
        mounted() {
            console.log('mounted');
            this.changeHand();
        },
        updated() {
            console.log('updated');
        },
        beforeDestroy() {
            clearInterval(interval);
        },
        destroyed() {
            console.log('destroyed');
        },
    }

</script>
<style scoped>
  #computer {
    width: 142px;
    height: 200px;
    background-position: 0 0;
  }
</style>