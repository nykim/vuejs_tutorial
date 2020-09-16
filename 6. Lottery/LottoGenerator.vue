<template>
    <div>
        <div>Generated numbers</div>
        <div id="resultWindow">
            <lotto-ball v-for="ball in winBalls" :key="ball" v-bind:number="ball "></lotto-ball>
        </div>
        <div>Bonus</div>
        <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
        <button v-if="redo" @click="onClickRedo">One more time!</button>
    </div>
</template>

<script>
import LottoBall from './LottoBall.vue';
function getWinningNumbers() {
    const candidate = Array(45).fill().map((v,i) => i + 1);
    const shuffle = [];
    while (candidate.length > 0) {
        shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
    }
    const bonusNumber = shuffle[shuffle.length - 1];
    const winningNumbers = shuffle.slice(0, 6).sort((p,c) => p - c);
    return [...winningNumbers, bonusNumber];
}

const timeouts = [];
export default {
    components: {
        'lotto-ball' : LottoBall,
    },
    data() {
        return {
            winningNumbers: getWinningNumbers(),
            winBalls: [],
            bonus: null,
            redo: false,
        }
    },
    computed: {
        
    },
    methods: {
        onClickRedo() {
            this.winningNumbers = getWinningNumbers();
            this.winBalls = [];
            this.bonus = null;
            this.redo = false;
            
        },
        showBalls() {
            for (let i=0; i < this.winningNumbers.length -1; i++) {
                timeouts[i] = setTimeout(()=> {
                    this.winBalls.push(this.winningNumbers[i]);
                }, (i + 1) * 1000);
            }
            timeouts[6] = setTimeout(() => {
                this.bonus = this.winningNumbers[6];
                this.redo = true;
            }, 7000);

        }

    },
    mounted() {
        this.showBalls();
    },
    beforeDestroy() {
        timeouts.forEach((t) => {
            clearTimeout(t);
        });
    },
    watch: {
        bonus(value, oldValue) {
            if (value === null) {
                this.showBalls();
            }
        },
    },
}
</script>

<style scoped>

</style>