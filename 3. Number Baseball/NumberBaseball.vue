<template>
    <div>
        <h1><u>Number Baseball Game</u></h1>
        <h2><font color="red">{{result}}</font></h2>
        <form @submit.prevent="onSubmitForm">
            <input ref="answer" minlength="4" maxlength="4" v-model="value" />
            <button type="submit">Submit</button>
        </form>
        <div>Number of tries: {{tries.length}}</div>
        <ul>
            <li v-for="t in tries">
                <div>{{t.try}}</div>
                <div>{{t.result}}</div>
            </li>
        </ul>
        <template v-if="result.length">
            <form @submit.prevent="onReset">
                <button type="submit">Restart</button>
            </form>
        </template>
    </div>
</template>

<script>

    const getNumbers = () => {
        const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
        const array = [];
        for (let i = 0; i < 4; i += 1) {
            const chosen = candidates.splice(Math.floor(Math.random() * (9-i)), 1)[0];
            array.push(chosen);
        }
        console.log('The answer is' + array);
        return array;
    };

    export default {
        data() {
            return {
                answer: getNumbers(),
                tries: [],
                value: '',
                result: '',
            }
        },
        methods: {
            onSubmitForm(e) {
                if (this.value === this.answer.join('')) {
                    this.result = `Homerun! The answer was ${this.answer.join(',')}!`;
                } else {
                    if  (this.tries.length >= 9) {
                        this.result = `10 tries and failed! The answer was ${this.answer.join(',')}!`;
                    }
                }

                let strike = 0;
                let ball = 0;
                const answerArray = this.value.split('').map(v => parseInt(v));
                console.log('answerArray' + answerArray);
                for (let i = 0; i < 4; i += 1) {
                    let num = answerArray[i];
                    if (num === this.answer[i]) {
                        console.log('strike! for' + this.answer[i]);
                        strike++;
                    } else if (this.answer.includes(num)) {
                        console.log('ball! for' + num);
                        ball++;
                    }
                }
                this.tries.push({
                    try: this.value,
                    result: `${strike} Strikes, ${ball} Balls.`,
                })

                this.value = '';
                this.$refs.answer.focus();
            },

            onReset() {
                this.answer = getNumbers();
                this.value = '';
                this.tries = [];
                this.result = '';
                alert('Game is restarting');
                this.$refs.answer.focus();
            }
        },
    }
</script>

<style>

</style>