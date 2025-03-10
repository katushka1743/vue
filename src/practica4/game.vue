<!-- Фильтры, нельзя нажимать в игре кнопки -->

<script setup>
import { reactive } from 'vue';
import modal from '../modal/modal.vue';

const props = defineProps({
    messageSettingsGame: {},
})

const emit = defineEmits([
    'GameResultEmits', 'GamePageEmits', 'GameStatusEmits'
])

const localGame = reactive({
    targets: [],
    result: 60,
    timerInterval: null, // Переменная для хранения интервала таймера
    targetMove: null,
    start: false,
    snitch: 0,
    status: false,

    modalShow: false, // переменная для показа модального окна
    modalText: '', // переменная для текста модального окна
    modalType: '', // переменная для типа модального окна

})


function startGame() {
    if (!localGame.start) {
        localGame.start = true
        localGame.status = true
        emit('GameStatusEmits', localGame.status)

        for (let index = 0; index < props.messageSettingsGame.count; index++) {
            localGame.targets.push({
                display: 'block',
                transform: `translateX(${Math.random() * 700}px) translateY(${Math.random() * 400}px)`,
                width: props.messageSettingsGame.size + 'px',
                height: props.messageSettingsGame.size + 'px'
            })
        }
        localGame.targetMove = setInterval(changePos, props.messageSettingsGame.speed * 100)

        // !!!
        localGame.timerInterval = setInterval(() => {
            if (localGame.result > 0 && localGame.targets.length > 0) {
                localGame.result -= 1;
            }
            if (localGame.result == 0 && localGame.targets.length > 0) {
                finnish('Время истекло, а цели остались. Увы, вы проиграли(')
            }
            if (localGame.result > 0 && localGame.targets.length == 0) {
                finnish(`Вы успели нажать на все цели. Ура, вы победили)\n Ваш результат: ${localGame.result}`)

            }
        }, 1000)
    }
}

function endGame() {
    localGame.result = 0
    finnish('Вы слабы(')
}

function modalGameOk(params) {
    localGame.modalShow = false
    emit('GamePageEmits')
    emit('GameResultEmits', localGame.result)
}

function finnish(modalMess) {
    clearInterval(localGame.targetMove)
    clearInterval(localGame.timerInterval)
    localGame.modalShow = true
    localGame.modalType = 'yes'
    localGame.modalText = modalMess
    localGame.targets = [];
    localGame.start = false
    localGame.status = false
    emit('GameStatusEmits', localGame.status)
}


function changePos() {
    localGame.targets.forEach(element => {
        element.transform = `translateX(${Math.random() * 700}px) translateY(${Math.random() * 400}px) `
    })
}



function snitched(index) {
    // localGame.targets.splice(index, 1);
    localGame.targets[index].display = 'none'
    localGame.result += 2
    localGame.snitch += 1
    if (localGame.snitch == props.messageSettingsGame.count) {
        clearInterval(localGame.targetMove)
        clearInterval(localGame.timerInterval)
        localGame.modalShow = true
        localGame.modalType = 'yes'
        localGame.modalText = `Вы успели нажать на все цели. Ура, вы победили)\n Ваш результат: ${localGame.result}`
        localGame.targets = [];
        localGame.start = false
        localGame.status = false
        emit('GameStatusEmits', localGame.status)
    }
}

</script>


<template>
    <h1>Игра</h1>
    <div class="game">
        <div v-for="(target, index) in localGame.targets" class="target"
            :style="{ display: target.display, transform: target.transform, width: target.width, height: target.height }"
            @click="snitched(index)">
        </div>

    </div>
    <button @click="startGame" v-if="!localGame.start">Начать</button>
    <button @click="endGame" v-if="localGame.start">Я слаб</button>
    <div>Количество секунд: {{ localGame.result }}</div>


    <!-- Передача в компонент modal:
		modalType - тип модального окна
		modalText - текст модального окна
	-->
    <!-- Получение из компонента modal emit modalOK и скрытие модального окна -->
    <modal v-if="localGame.modalShow" :modalType="localGame.modalType" :modalText="localGame.modalText"
        @modalOK="modalGameOk" />
</template>


<style scoped>
.game {
    border: 1px solid salmon;
    width: 800px;
    height: 500px;
    text-align: left;
    position: relative;
}

.target {
    background: palevioletred;
    position: absolute;
    transition: 1s;
}
</style>
