<template>
    <div class="board-wrapper">
        <div class="board">
            <BoardItem
                v-for="field in fields"
                :field="field"
                :gameStatus="gameStatus"
                :key="'item-' + field.id"
                @selectField="selectField($event)"
            />
        </div>
        <p class="difficult">
            Difficulty: <strong>{{ difficult }}</strong>
        </p>
        <p class="win" v-if="isWin">
            Поздравляю! Следующий уровень!
        </p>
        <p class="fail" v-if="isFail">
            Возможно в следующий раз повезет :)
        </p>
        <button class="btn" @click="start" :disabled="!canStartGame">
            Start
        </button>
    </div>
</template>

<script>
import BoardItem from "./BoardItem";
import useGameInit from "@/components/composables/useGameInit";
import useGameStart from "@/components/composables/useGameStart";
import useGameProcess from "@/components/composables/useGameProcess";
import { GAME_STATUS } from "@/constants";
import { ref } from "vue";

export default {
    name: "Board",
    props: {},
    components: {
        BoardItem,
    },
    setup() {
        let number = 25;
        
        const gameStatus = ref(GAME_STATUS.NONE);

        const { difficult, fields, init } = useGameInit(number);

        const { start, canStartGame } = useGameStart(
            fields,
            init,
            difficult,
            number,
            gameStatus,
        );

        const { selectField, isWin, isFail } = useGameProcess(fields, gameStatus, difficult, start);

        return {
            number,
            difficult,
            fields,
            init,
            start,
            gameStatus,
            canStartGame,
            selectField,
            isWin,
            isFail
        };
    },
};
</script>

<style scoped>
.board-wrapper {
    margin-bottom: 100px;
}

.board {
    width: 300px;
    margin: 0 auto;
    background: #eee;
}

.btn {
    color: #fff;
    margin: 10px 0;
    padding: 10px 30px;
    background: rgba(66, 185, 131, 0.8);
    border: none;
    border-radius: 2px;
    cursor: pointer;
    outline: none;
}

.btn:hover {
    background: rgba(66, 185, 131, 1);
}

.btn:disabled {
    opacity: .5;
}

.win {
    font-size: 18px;
    color: #42b983;
}

.fail {
    font-size: 18px;
    color: rgb(230, 19, 19);
}
</style>