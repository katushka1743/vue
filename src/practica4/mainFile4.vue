<script setup>
import { reactive } from 'vue';
import settings from './settings.vue';
import game from './game.vue';
import statistic from './statistic.vue';
import modal from '../modal/modal.vue';

const localMain = reactive({
    curPage: 1, // счётчик страниц
    settingsEm: {},
    settingsEms: [],
    modalShow: false, // переменная для показа модального окна
    modalText: '', // переменная для текста модального окна
    modalType: '', // переменная для типа модального окна
    status: false,
})

function gameResEmit(result) {
    if (localMain.settingsEms.length > 0) {
        localMain.settingsEms[localMain.settingsEms.length - 1].res = result;
    }
}

function saveSettingsEmits(params) {
    localMain.settingsEms.push({
        count: params.count,
        speed: params.speed,
        size: params.size,
        res: null,
    });
    // localMain.settingsEm = params

    localMain.settingsEm = ({
        count: params.count,
        speed: params.speed,
        size: params.size,
        res: null,
    });


    // console.log(localMain.settingsEms);

}

function GameCheck(params) {
    localMain.modalType = 'yes'
    localMain.modalText = 'Без выбора настроек вы не можете перейти на страницу игры'
    localMain.modalShow = true
    localMain.curPage = 1
}

function GameStatus(params) {
    localMain.status = params
}


</script>


<template>
    <div class="navigation">
        <button @click="localMain.curPage = 1" :disabled='localMain.status'
            :class="{ active_nav: localMain.curPage == 1 }">Настройки</button>
        <button @click="GameCheck" :disabled='localMain.status'
            :class="{ active_nav: localMain.curPage == 2 }">Игра</button>
        <button @click="localMain.curPage = 3" :disabled='localMain.status'
            :class="{ active_nav: localMain.curPage == 3 }">Статистика</button>
    </div>


    <settings v-if="localMain.curPage == 1" @settingsEmit="saveSettingsEmits"
        @settingsEmitPage="localMain.curPage = 2" />

    <game v-if="localMain.curPage == 2" :messageSettingsGame="localMain.settingsEm"
        @GamePageEmits="localMain.curPage = 3" @GameResultEmits="gameResEmit" @GameStatusEmits="GameStatus" />


    <statistic v-if="localMain.curPage == 3" :messageSettings="localMain.settingsEms" />

    <!-- Передача в компонент modal:
		modalType - тип модального окна
		modalText - текст модального окна
	-->
    <!-- Получение из компонента modal emit modalOK и скрытие модального окна -->
    <modal v-if="localMain.modalShow" :modalType="localMain.modalType" :modalText="localMain.modalText"
        @modalOK="localMain.modalShow = false" />



</template>


<style scoped>
.navigation {
    min-width: 800px;
    justify-content: space-around;
    position: fixed;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    padding: 10px;
    background-color: #242424;
}

.active_nav {
    background-color: blueviolet;
    border: solid 1px pink;
}
</style>