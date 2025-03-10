<script setup>
import { onMounted, reactive, onUnmounted } from 'vue';
// import { onMounted, onUnmounted } from 'vue';

const localSettings = reactive({
    setSettings: {
        count: 0,
        speed: 0,
        size: 0,
        res: null,
    },
    error: false
})

const emit = defineEmits([
    'settingsEmit', 'settingsEmitPage'
])

function saveSettings() {
    if (localSettings.setSettings.count != 0 && localSettings.setSettings.speed != 0 && localSettings.setSettings.size != 0) {
        emit('settingsEmit', localSettings.setSettings)
        emit('settingsEmitPage')
        localSettings.error = false
        localSettings.setSettings.count = 0
        localSettings.setSettings.speed = 0
        localSettings.setSettings.size = 0
    }
    else {
        localSettings.error = true
    }

}


onMounted(() => {
    console.log('Я родился')
})

onUnmounted(() => {
    console.log('Я умер')

})


</script>


<template>
    <h1>Настройки игры</h1>
    <h2>Выберите режим игры</h2>
    <div class="choice">
        <div class="count">
            <h3>Количество целей</h3>
            <div class="buttons">
                <button @click="localSettings.setSettings.count = 5"
                    :class="{ choice_active: localSettings.setSettings.count == 5 }">5</button>
                <button @click="localSettings.setSettings.count = 10"
                    :class="{ choice_active: localSettings.setSettings.count == 10 }">10</button>
                <button @click="localSettings.setSettings.count = 15"
                    :class="{ choice_active: localSettings.setSettings.count == 15 }">15</button>
            </div>
        </div>
        <div class="speed">
            <h3>Скорость целей</h3>
            <div class="buttons">
                <button @click="localSettings.setSettings.speed = 10"
                    :class="{ choice_active: localSettings.setSettings.speed == 10 }">Низкая (10s)</button>
                <button @click="localSettings.setSettings.speed = 7"
                    :class="{ choice_active: localSettings.setSettings.speed == 7 }">Средняя (7s)</button>
                <button @click="localSettings.setSettings.speed = 5"
                    :class="{ choice_active: localSettings.setSettings.speed == 5 }">Высокая (5s)</button>
            </div>
        </div>
        <div class="size">
            <h3>Размер целей</h3>
            <div class="buttons">
                <button @click="localSettings.setSettings.size = 50"
                    :class="{ choice_active: localSettings.setSettings.size == 50 }">Крупный (50px)</button>
                <button @click="localSettings.setSettings.size = 40"
                    :class="{ choice_active: localSettings.setSettings.size == 40 }">Средний (40px)</button>
                <button @click="localSettings.setSettings.size = 25"
                    :class="{ choice_active: localSettings.setSettings.size == 25 }">Маленький (25px)</button>
            </div>
        </div>
    </div>
    <br>
    <button @click="saveSettings">Сохранить настройки</button>
    <div v-if="localSettings.error == true" class="error">Нужно выбрать все значения</div>

    <!-- <button style="width: 50px; height: 50px; background-color: palevioletred; padding: 0; margin: 0;"></button><br>
    <button style="width: 40px; height: 40px; background-color: palevioletred; padding: 0; margin: 0;"></button><br>
    <button style="width: 25px; height: 25px; background-color: palevioletred; padding: 0; margin: 0;"></button> -->

</template>


<style scoped>
.choice {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
}

.buttons {
    display: flex;
    flex-direction: column;
}

.choice_active {
    background-color: blueviolet;
    border: solid 1px pink;
}

.error {
    color: red;
}
</style>