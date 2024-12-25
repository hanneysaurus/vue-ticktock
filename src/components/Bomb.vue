<template>
  <img
      src="../assets/bomb.png"
      alt="bomb"/>
  <button ref="button" @click="handleClick">{{ isTicking ? 'STOP' : 'START' }}</button>
</template>

<script lang="js" setup>

import {ref} from "vue";

const isTicking = ref(false);
const initialPlaybackrate = 1.0;
const finalPlaybackRate = 2.0;
let time = 0;

const tickingAudio = new Audio('/ticking.mp3');
const explosionAudio = new Audio('/explosion.mp3');

const handleClick = () => {
  console.log("IS TICKING "+ isTicking.value);
  if (isTicking.value) {
    endBomb();
    console.log("ending bomb");
  } else {
    startBomb();
    console.log("starting bomb");
  }
}

const startBomb = () => {

  isTicking.value = true;

  // times in seconds
  const minimumTime = 15;
  const maximumTime = 45;
  time = Math.floor(Math.random() * (maximumTime - minimumTime + 1)) + minimumTime;

  let playbackRate = 0.9;
  let playbackRateIncrease = (finalPlaybackRate / (time));

  tickingAudio.loop = true;
  tickingAudio.play();

  setInterval(() => {
    playbackRate += playbackRateIncrease;
    tickingAudio.playbackRate = playbackRate;
  }, 1000);

  endBomb();
}

const endBomb = () => {
  isTicking.value = false;

  setTimeout(() => {
    tickingAudio.pause();
    tickingAudio.currentTime = 0;
    tickingAudio.playbackRate = initialPlaybackrate;
    explosionAudio.play();
  }, 1000 * time);
}

</script>

<style scoped>

</style>
