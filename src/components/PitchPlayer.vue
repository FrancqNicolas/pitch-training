<script setup>
import { Howl } from 'howler';
import { computed, watch, ref } from 'vue';

const props = defineProps({
    notes: Array,
    scales: Number
})

let playedNote = ref('')

const getRandomNote = computed(() => {
    const scaleRange = {min: 1, between: 4, max: 7}
    const scaleInRange = []
    let lower = scaleRange.between
    let higher = scaleRange.between

    for (let i = 0; i < props.scales; i++) {
        if(i === 0) {
            scaleInRange.push(scaleRange.between)
        }
        else {
            if(lower > scaleRange.min && i % 2 === 0) {
                lower = lower - 1
                scaleInRange.push(lower)
            }

            if(higher < scaleRange.max && i % 2 !== 0) {
                higher = higher + 1
                scaleInRange.push(higher)
            }
        }
    }

    playedNote = props.notes[Math.floor(Math.random()*props.notes.length)]

    return props.notes[Math.floor(Math.random()*props.notes.length)] + scaleInRange[Math.floor(Math.random()*scaleInRange.length)]
});

const guessNote = (note) => {
    buttonClass.value = note === playedNote ? "success" : "error"
}

let playNote = new Howl({
  src: [require(`@/assets/notes/${getRandomNote.value}.mp3`)]
});

watch(props, () => {
    playNote = new Howl({
        src: [require(`@/assets/notes/${getRandomNote.value}.mp3`)]
    });
})

</script>

<template>
    <div id="hear">
        <button @click="playNote.play()">Hear note</button>
    </div>
    <div>
        <button @click="guessNote(note)" :class="buttonClass" v-for="note in props.notes" :key="note">
            {{ note }}
        </button>
    </div>
</template>

<style>
</style>