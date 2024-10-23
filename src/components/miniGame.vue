<template>
    <h1>Word Master</h1>
    <div class="input-row">
        <div class="boxWrapper">
            <input v-for="(letter, index) in currentGuess" 
            :key="index"
            v-model="currentGuess[index]"
            @keyup.enter="submitGuess"
            maxlength="1"
            placeholder=""
            class="inputClass"
        /> 
        </div>
        <div class="board">
            <div v-for="(row, rowIndex) in guesses" :key="rowIndex" class="letterWrapper">
                <div v-for="(letter, index) in row" :key="index" :class="['cell', letter.class]">
                    {{ letter.char }}
                </div>
            </div>
        </div>
        <div v-if="gameOver">
            <h2>{{ gameResult }}</h2>
            <button @click="restart"> Play Again! </button>
        </div>
    </div>
</template>
<script>
    import { ref } from 'vue';
    export default {
        setup() {
            const targetWord = "APPLE";
            const maxAttempst = 6;
            const currentGuess = ref(['', '', '', '', '']);
            const guesses = ref([]);
            const gameOver = ref(false);
            const gameResult = ref([]);

            const submitGuess = () => {
                const formattedGuess = currentGuess.value.join('').toUpperCase();
                if (formattedGuess.length < 5) return;

                const result = [];
                for (let i = 0; i < 5; i++) {
                    const letter = formattedGuess[i];
                    if (letter === targetWord[i]) {
                        result.push ({char: letter, class: 'correct'});
                    } else if (targetWord.includes(letter)) {
                        result.push ({char: letter, class: 'present'});
                    } else {
                        result.push ({char: letter, class: 'absent'});
                    }
                }
                guesses.value.push(result); 
                currentGuess.value = ['', '', '', '', ''];
                if (formattedGuess === targetWord) {
                    gameOver.value = true;
                } else if (guesses.value.length >= maxAttempst) {
                    gameResult.value = `Game over! the word was: ${targetWord}`;
                    gameOver.value = true;
                }
            }

            const restart = () => {
                currentGuess.value = ['', '', '', '', ''];
                guesses.value = [];
                gameOver.value = false;
                gameResult.value = '';
            }

            return {
                currentGuess,
                guesses,
                gameResult,
                gameOver,
                submitGuess,
                restart
            }
        }
    }
</script>
<style scoped>
    .boxWrapper {
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
    }
    .inputClass {
        width: 50px;
        height: 50px;
        font-size: 30px;
    }
    .letterWrapper {
        display: flex;
        justify-content: center;
        margin: 20px;
        font-size: 30px;
    }
    .correct {
        color: green;
    }
    .absent {
        color: red;
    }
    .present {
        color: yellow;
    }
</style>