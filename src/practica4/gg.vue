<!-- Работающий с ИИ: -->

<script setup>
import { reactive } from 'vue';

const localGame = reactive({
    targets: [],
    time_result: 60,
    targetMove: null,
    start: false,

})

const props = defineProps({
    messageSettingsGame: {},
})


function startGame() {

    if (!localGame.start) {
        localGame.time_result = 60;
        localGame.targets = [];

        for (let index = 0; index < props.messageSettingsGame.count; index++) {
            localGame.targets.push({
                display: 'block',
                transform: `translateX(${Math.random() * 800}px) translateY(${Math.random() * 500}px)`,
                width: props.messageSettingsGame.size + 'px',
                height: props.messageSettingsGame.size + 'px'
        })
        }
    localGame.targetMove = setInterval(changePos, props.messageSettingsGame.speed * 100)
    localGame.start = true
    }
}

function stopGame(params) {
    clearInterval(localGame.targetMove)
    localGame.start = false
}

function changePos(index) {
    
    localGame.targets.forEach(element => {
        element.transform = `translateX(${Math.random() * 800}px) translateY(${Math.random() * 500}px) `
    })


}


function snitch(index) {
    localGame.targets.splice(index, 1);
}

</script>


<template>
    <h1>Игра</h1>
<!-- 
    Количество целей: {{ props.messageSettingsGame.count }} 
    Скорость целей: {{ props.messageSettingsGame.speed }}
    Размер целей: {{ props.messageSettingsGame.size }}
    Результат: {{ props.messageSettingsGame.res }} 
-->


    <div class="game">
        <!-- <div v-for="(elem, index) in localGame.targets" :key="index"
        class="target"
        :style="{display: elem.display, transform: elem.transform, width: elem.width, height: elem.height}"
        @click="snitch(index)"
        ></div> -->
        <div v-for="(target, index) in localGame.targets" :key="index"
            class="target"
            :style="{ display: target.display, transform: target.transform, width: target.width, height: target.height }"
            @click="snitch(index)">
        </div>
    
    </div>
    <button @click="startGame">Начать</button>
    <button @click="stopGame">Сдаться</button>
    <div>Количество секунд: {{ localGame.time_result }}</div>
</template>


<style scoped>
    .game {
        border: 1px solid salmon ;
        width: 65vw;
        height: 65vh;
        text-align: left;

    }

    .target{
        background: palevioletred;
        position: absolute;
    }



</style>





<!-- Изначально: -->

<!-- 

<script setup>
import { reactive } from 'vue';

const localGame = reactive({
    targets: [],
    time_result: 60,
    targetMove: null,
    start: false,

})

const props = defineProps({
    messageSettingsGame: {},
})

const styleGame = reactive ({
    transform: null,
    display: 'block',
    // width: props.messageSettingsGame.size,
    // height: props.messageSettingsGame.size,
})

// localGame.targetMove = setInterval(changePos, props.messageSettingsGame.speed)
// clearInterval(localGame.targetMove)



function startGame() {

    if (!localGame.start) {
        localGame.targets = [];

        localGame.time_result -=1
        // console.log(props.messageSettingsGame.count);
        for (let index = 0; index < props.messageSettingsGame.count; index++) {
            localGame.targets.push({
                display: 'block',
                transform: `translateX(${Math.random() * 800}px) translateY(${Math.random() * 500}px)`,
                width: props.messageSettingsGame.size + 'px',
                height: props.messageSettingsGame.size + 'px'
        })
        // console.log(localGame.targets);
        
        }
    

    localGame.targetMove = setInterval(changePos, props.messageSettingsGame.speed * 100)
    localGame.start = true


    }
}

function stopGame(params) {
    clearInterval(localGame.targetMove)
    localGame.start = false
}

function changePos(index) {
    // styleGame.transform = `translateX(${Math.random() * 800}px) translateY(${Math.random() * 500}px) `
    // styleGame.transform = `translateX(${Math.random() * 65}vw) translateY(${Math.random() * 65}vh) `
    localGame.targets.forEach(element => {
        // element.transform = `translateX(${Math.random() * 65}vw) translateY(${Math.random() * 65}vh) `
        element.transform = `translateX(${Math.random() * 800}px) translateY(${Math.random() * 500}px) `
        

    })


}


function snitch(index) {
    // styleGame.display = 'none'
    // // props.messageSettingsGame.count -= 1
    // localGame.targets.splice(index, 1);

    // console.log(props.messageSettingsGame.count);

    localGame.targets.splice(index, 1);

}

</script>


<template>
    <h1>Игра</h1>
<!-- 
    Количество целей: {{ props.messageSettingsGame.count }} 
    Скорость целей: {{ props.messageSettingsGame.speed }}
    Размер целей: {{ props.messageSettingsGame.size }}
    Результат: {{ props.messageSettingsGame.res }} 
-->


    <!-- <div class="game">
        <div v-for="(elem, index) in localGame.targets" :key="index"
        class="target"
        :style="{display: elem.display, transform: elem.transform, width: elem.width, height: elem.height}"
        @click="snitch(index)"
        ></div>
        <div v-for="(target, index) in localGame.targets" :key="index"
            class="target"
            :style="{ display: target.display, transform: target.transform, width: target.width, height: target.height }"
            @click="snitch(index)">
        </div>
    
    </div>
    <button @click="startGame">Начать</button>
    <button @click="stopGame">Сдаться</button>
    <div>Количество секунд: {{ localGame.time_result }}</div>
</template> -->


<!-- <style scoped>
    .game {
        border: 1px solid salmon ;
        /* width: 800px;
        height: 500px; */
        width: 65vw;
        height: 65vh;
        text-align: left;
        position: relative;
    }

    .target{
        background: palevioletred;
        position: absolute;
    }



</style> --> 