<template>
    <header class="flex items-center justify-between ">
        <div class="grid-num">
            <h3 class="text-xs font-bold">Grid: {{ gridnumber }} X {{ gridnumber }}</h3>
        </div>
        <div class="life-bar">
            <div class="progress-bar relative">
                <div class="progress-value" :style="lifeBar" ></div>
                <div class="heart h-8 w-8 absolute -left-3 -top-2">
                    <svg hclass="h-full w-full" viewBox="0 0 53 47" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M51.8238 13.3213C43.4729 -9.03737 27.0688 6.58687 27.0688 6.58687C27.0688 6.58687 9.52649 -8.76792 1.98383 13.3214C-1.51815 29.4845 26.7712 45.1086 26.7712 45.1086C26.7712 45.1086 55.3258 29.4844 51.8238 13.3213Z" fill="#CE4557" stroke="#EE8D94" stroke-width="1.6163"/>
                    <path d="M13.029 9.27065C11.0292 10.8191 10.1408 12.0215 9.13881 10.7275C8.13685 9.43356 8.94573 7.1293 10.9455 5.58081C12.9453 4.03233 16.6065 4.28686 17.6084 5.58082C18.6104 6.87478 15.0288 7.72217 13.029 9.27065Z" fill="white"/>
                    <path d="M7.64138 12.9675C7.64138 13.5626 6.58914 14.3144 5.99404 14.3144C5.39893 14.3144 4.9165 13.832 4.9165 13.2369C4.9165 12.6418 6.47647 11.0818 7.07157 11.0818C7.66668 11.0818 7.64138 12.3724 7.64138 12.9675Z" fill="white"/>
                    </svg>
                </div>
            </div>
        </div>
        <div class="time">
            <p class="text-xs ">Time spent: {{ timeMinutes }} mins :{{ timeSeconds }} secs</p>
        </div>
    </header>
</template>

<script>
export default {
    props: ['gridnumber'],
    data(){
        return{
            count: 0,
            timeStop: null,
        }
    },
    methods:{
        incrementCount(){
            this.count++;
        }
    },
    computed:{
        timeSeconds(){
            return this.count % 60;
        },
        timeMinutes(){
            return Math.floor((this.count / 60));
        },
        moves(){
            return this.$store.getters['getMoves'];
        },
        maxMoves(){
            return this.$store.getters['getMaxMoves'];
        },
        lifeBar(){
            return `width: ${this.lifeBarValue}%`;
        },
        lifeBarValue(){
            let value = 100 - ((this.moves / this.maxMoves) * 100);
            if(value > 0){
                return value;
            }
            else{
                return 0;
            }
        }
    },
    mounted(){
        this.timeStop = setInterval(this.incrementCount, 1000);
    },
    unmounted(){
        const timeSpent = `Time Spent: ${this.timeSeconds} seconds`;
        this.$store.dispatch({
            type: 'timeSpentUpdate',
            value: timeSpent
        });
       
        clearInterval(this.timeStop);
    }
}
</script>

<style scoped>
    .grid-num{
        flex: 0 0 24.34%;
    }
    .life-bar{
        flex: 0 0 22.33%;
    }
    .time{
        flex: 0 0 33.33%;
    }
    .progress-bar{
        height: .7rem;
        width: 100%;
        background: linear-gradient(180deg, #853594 47.39%, #692789 47.4%);
        border-radius: .5rem;
    }

    .progress-value{
        width: 100%;
        border-radius: .5rem;
        height: 100%;
        background: linear-gradient(180deg, #F07CC3 44.27%, #C5418E 44.28%);
    }

    header{
        height: 10%;
    }
 
</style>