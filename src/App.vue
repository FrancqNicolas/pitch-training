<script setup>
import { ref, watch } from 'vue';
import PitchPlayer from './components/PitchPlayer.vue';
import PitchOptions from './components/PitchOptions.vue';

const notes = ref(["C"])
const scales = ref(3)

</script>

<template>
  <main>
    <section id="player">
      <PitchPlayer :notes="notes" :scales="scales" />
    </section>

    <section id="options">
      <PitchOptions 
        :startNotes="notes"
        :startScales="scales"
        @update-scales="s => scales = s"
        @update-notes="n => notes = n"
      />
    </section>
  </main>
</template>

<style lang="scss">
@import "assets/css/style.css";

main {
  height: 100%;
  width: 100%;
}

section {
  display: flex;
  justify-content: center;
  align-items: center;
}

#player {
    height: 400px;
}

#options {
  flex-wrap: wrap;
}
</style>