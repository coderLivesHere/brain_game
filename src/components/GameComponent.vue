<template>
    <div class="p-2 bg-gray-200 shadow-lg rounded-lg">
        <ul id="GameMenu"
            class="flex justify-between mb-2">
            <li class="flex items-center">
                <label for="SelectLevel">
                    Уровень:
                </label>
                <select id="SelectLevel" :disabled="gameStarted" v-model="selectLevel" class="w-[80px] border border-gray-700 ml-1 rounded bg-white">
                    <option selected value="easy">
                        легко
                    </option>
                    <option value="medium">
                        средне
                    </option>
                    <option value="hard">
                        сложно
                    </option>
                </select>
            </li>
            <li class="flex items-center">
                Этап: {{ stage }}
            </li>
            <li class="flex items-center">
                <button @click="start()" class="px-2 py-1 text-white bg-blue-600 hover:opacity-80" :class="{'hidden': gameStarted}">
                    Старт
                </button>
                <button @click="stop()" class="px-2 py-1 text-white bg-red-600 hover:opacity-80" :class="{'hidden': !gameStarted}">
                    Стоп
                </button>
            </li>
        </ul>
        <ul id="PlayingField"
            class="grid gap-2" :class="{'grid-cols-3 grid-rows-3': selectLevel === 'easy', 'grid-cols-4 grid-rows-4': selectLevel === 'medium', 'grid-cols-5 grid-rows-5': selectLevel === 'hard'}">
            <template v-if="selectLevel === 'easy'">
                <li class="col-span-1 w-24 h-24" v-for="item in 9" :key="item.key">
                    <button @click="clickBtn" class="btn-game w-full h-full hover:opacity-80 pointer-events-none" v-bind:data-id="item" v-bind:style="'background-color:' + this.colors[item] + ';'"></button>
                </li>
            </template>
            <template v-if="selectLevel === 'medium'">
                <li class="col-span-1 w-24 h-24" v-for="item in 16" :key="item.key">
                    <button @click="clickBtn" class="btn-game w-full h-full hover:opacity-80 pointer-events-none" v-bind:data-id="item" v-bind:style="'background-color:' + this.colors[item] + ';'"></button>
                </li>
            </template> 
            <template v-if="selectLevel === 'hard'">
                <li class="col-span-1 w-24 h-24" v-for="item in 25" :key="item.key">
                    <button @click="clickBtn" class="btn-game w-full h-full hover:opacity-80 pointer-events-none" v-bind:data-id="item" v-bind:style="'background-color:' + this.colors[item] + ';'"></button>
                </li>
            </template>  
        </ul>
    </div>
</template>

<script>
export default {
  name: 'GameComponent',
  data() {
    return {
        selectLevel: 'easy',
        gameStarted: false,
        sequenceButtons: [],
        setTimeouts: [],
        allGameBtn: null,
        stage: 0,
        colors: [
            'none',
            '#0d9488',
            '#0284c7',
            '#2563eb',
            '#4f46e5',
            '#7c3aed',
            '#dc2626',
            '#ea580c',
            '#ca8a04',
            '#16a34a',
            '#059669',
            '#818cf8',
            '#60a5fa',
            '#38bdf8',
            '#22d3ee',
            '#2dd4bf',
            '#34d399',
            '#4ade80',
            '#a3e635',
            '#facc15',
            '#57534e',
            '#9333ea',
            '#c026d3',
            '#db2777',
            '#e11d48',
            '#fb7185',
        ],
    }
  },
  methods: {
        start() 
        {
            this.sequenceButtons = [];
            this.stage = 0;
            this.gameStarted = true;
            this.allGameBtn = document.querySelectorAll('.btn-game');
            this.game();
        },
        game()
        {
            this.stage += 1;
            for(let i = 0; i <= this.stage; i++)
            {
                this.sequenceButtons.push(Math.ceil(Math.random() * this.allGameBtn.length));
            }
            for(let i = 1; i <= this.sequenceButtons.length; i++)
            {
                let x = i * 2000;
                this.setTimeouts.push(setTimeout(() => {this.allGameBtn[this.sequenceButtons[i - 1] - 1].classList.add('opacity-40'); this.selectBtnSoundPlay()}, x - 1000));
                this.setTimeouts.push(setTimeout(() => {this.allGameBtn[this.sequenceButtons[i - 1] - 1].classList.remove('opacity-40')}, x));
                this.setTimeouts.push(setTimeout(() => {this.allGameBtn.forEach(element => {element.classList.remove('pointer-events-none')})}, x + (2000 * this.stage) + 500));
            }
        },
        stop() 
        {
            this.allGameBtn.forEach(element => {element.classList.add('pointer-events-none')});
            this.allGameBtn.forEach(element => {element.classList.remove('opacity-40')});
            this.setTimeouts.forEach(element => {clearTimeout(element)})
            this.gameStarted = false;
            this.endGameAlert();
        },
        clickBtn(event)
        {
            this.clickBtnSoundPlay();
            if (event.target.dataset.id == this.sequenceButtons[0])
            {
                this.sequenceButtons.shift();
                this.checkingSequenceButtons();
            } else
            {
                this.stop();
            }
        },
        checkingSequenceButtons()
        {
            if (this.sequenceButtons.length === 0)
            {
                this.setTimeouts.forEach(element => {clearTimeout(element)});
                this.allGameBtn.forEach(element => {element.classList.add('pointer-events-none')});
                this.game();
            }
        },
        endGameAlert()
        {
            if(this.stage <= 1)
            {
                alert('Серьёзно???');
            }
            else if(this.stage <= 2)
            {
                alert('Ну такое...');
            }
            else if(this.stage <= 5)
            {
                alert('Могло быть и лучше :(');
            }
            else if(this.stage <= 10)
            {
                alert('Способный игрок :)');
            }
            else if(this.stage <= 20)
            {
                alert('Почти крут :)');
            }
            else
            {
                alert('Монстр!!!');
            }
        },
        selectBtnSoundPlay()
        {
            let audio = new Audio(require('@/assets/sounds/select.mp3'));
            audio.play();
        },
        clickBtnSoundPlay()
        {
            let audio = new Audio(require('@/assets/sounds/click.mp3'));
            audio.play();
        },
    }
}
</script>

<style scoped>
</style>