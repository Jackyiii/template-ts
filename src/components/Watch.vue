<template>
    <div class="circle">
        <div class="dial" :class="showShining ? 'light': ''" @onclick="handleClick">
            {{observer.hour}}
            <span style="font-size: 12px">{{seconds}}</span>
        </div>
    </div>

    <button @click="shining" style="position: absolute; top: 18rem; left: 2rem;background: #b5fbcb;
    padding: 6px 12px;">Light</button>
    <button @click="handleClick" style="position: absolute; top: 9rem; left: 20rem; padding: 6px 12px;">Mode</button>
    <button @click="handleIncrease" style="position: absolute; top: 16rem; left: 20rem; padding: 6px 12px;">Increase</button>
</template>

<script lang="ts">
import { Vue } from 'vue-class-component'
import { Subject, Observer } from '../utils/watch'
export default class HelloWorld extends Vue {

    subject: Subject = new Subject(new Date())
    clickState = 0
    observer: Observer = new Observer()
    increase = false
    showShining = false
    seconds = new Date().getSeconds()

    mounted(): void {
        this.subject.addObserver(this.observer)
        setTimeout(() => {
            this.seconds++
        }, 1000)
    }

    handleClick(): void {
        if (this.increase) {
            this.increase = false
            this.clickState = 0
        }
        this.clickState++
    }

    handleIncrease(): void {
        this.increase = true
        switch (this.clickState) {
            case 1:
                this.subject.addHour()
                break
            case 2:
                this.subject.addMinute()
                break
            case 3:
                break
            default:
                break
        }
    }

    shining() {
        this.showShining = !this.showShining
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.circle {
    width: 300px;
    height: 300px;
    background: #837d7d;
    border-radius: 50%;
    padding-top: 2px;
    display: flex;
    align-items: center;
    margin-left: 2rem;
}
.dial {
    width: 200px;
    height: 100px;
    background: #fff;
    border-radius: 12px;
    margin: 0 auto;
    align-items: center;
    font-size: 40px;
    line-height: 96px;
}
.light {
    background: yellow;
}
</style>
