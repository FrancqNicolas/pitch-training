<script setup>
import { Howl } from 'howler';
import { computed, watch } from 'vue';

const props = defineProps({
    notes: Array,
    scales: Number
})

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

    return props.notes[Math.floor(Math.random()*props.notes.length)] + scaleInRange[Math.floor(Math.random()*scaleInRange.length)]
});

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
    <button @click="playNote.play()">joue</button>
</template>