<script setup>
import { ref, watch, defineEmits } from 'vue';

const props = defineProps({
    startNotes: Array,
    startScales: Number
})

const chromaticScale = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B']
const activeNotes = ref(props.startNotes)
const scales = ref(props.startScales)
const emit = defineEmits(['updateScales', 'updateNotes']);

const toggleNote = (note) => {
    (activeNotes.value.includes(note))
        ? activeNotes.value = activeNotes.value.filter(n => n !== note)
        : activeNotes.value = [...activeNotes.value, note];
};

watch(scales, (s) => {
    emit('updateScales', s)
})

watch(activeNotes, (n) => {
    emit('updateNotes', n)
});

</script>

<template>
    <h2>Notes to be tested with</h2>
    <button @click="() => toggleNote(note)" v-for="note in chromaticScale" :key="note"
        :class="['piano-key', { 'sharp': note.includes('#'), 'active': activeNotes.includes(note) }]">
        {{ note }}
    </button>

    <h2>Scales</h2>
    <input v-model="scales" type="number" value="{{ props.startScales }}" min="1" max="7">
</template>

<style lang="scss">
.piano {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    gap: 1px;
}

.piano-key {
    text-align: center;
    padding: 2%;
}

.piano-key.sharp {
    grid-column: span 1;
    margin-bottom: 100px;
}

.piano-key.active {
    background: #0096bfab;
}

h2 {
    flex-basis: 100%;
}
</style>