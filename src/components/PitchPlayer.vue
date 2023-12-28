<script setup>
import { Howl } from 'howler';
import { computed, watch, ref } from 'vue';

const props = defineProps({
    notes: Array,
    scales: Number
})

let playedNote = ref('')
let selectedNote = ref('');

const checkNote = (note) => {
    return note === playedNote.value;
};

const selectNote = (note) => {
    selectedNote.value = note;

    if(note === playedNote.value) {
        setTimeout(() => {
            selectedNote.value = ''
            const newNote = getRandomNote()
            getNewNote(newNote)
        }, 500)
    }
};

const getRandomNote = () => {
    const scaleRange = {min: 1, between: 4, max: 7}
    const scaleInRange = []
    let lower = scaleRange.between
    let higher = scaleRange.between

    for (let i = 0; i < props.scales; i++) {
        if(i === 0) scaleInRange.push(scaleRange.between)
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

    playedNote.value = props.notes[Math.floor(Math.random()*props.notes.length)]
    return props.notes[Math.floor(Math.random()*props.notes.length)] + scaleInRange[Math.floor(Math.random()*scaleInRange.length)]
};

const getNewNote = (note) => {
    playNote = new Howl({
        src: [require(`@/assets/notes/${note}.mp3`)]
    });

    playedNote.value = note.charAt(0)

    playNote.play()
    console.log("fun")
}

let playNote = new Howl({
  src: [require(`@/assets/notes/${getRandomNote()}.mp3`)]
});

watch(props, () => {
    selectedNote.value = ''
    const newNote = getRandomNote()
    getNewNote(newNote)
})

</script>

<template>
    <div>
        <button @click="playNote.play()">Hear note</button>
    </div>
    <div>
        <button v-for="note in props.notes" :key="note" 
                :class="{ 'success': selectedNote === note && checkNote(note), 'error': selectedNote === note && !checkNote(note) }"
                @click="selectNote(note)">
            {{ note }}
        </button>
    </div>
</template>