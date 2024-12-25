<template>
  <div class="container">
    <img
        :src="imagePath"
        alt="bomb"
        class="image"/>
    <button class="activate-button" @click="handleClick">{{ isTicking ? 'STOP' : 'START' }}</button>
  </div>
</template>

<script lang="js" setup>

import {ref} from "vue";

const isTicking = ref(false);
const initialPlaybackrate = 1.0;
const finalPlaybackRate = 2.0;
const imagePath = ref('/bomb.png');
let time = 0;

const tickingAudio = new Audio('/ticking.mp3');
const explosionAudio = new Audio('/explosion.mp3');

const handleClick = () => {
  if (isTicking.value) {
    endBomb();
  } else {
    startBomb();
  }
}

const startBomb = () => {

  imagePath.value = '/bomb.png';

  // times in seconds
  const minimumTime = 15;
  const maximumTime = 45;
  time = Math.floor(Math.random() * (maximumTime - minimumTime + 1)) + minimumTime;

  let playbackRate = initialPlaybackrate;
  let playbackRateIncrease = (finalPlaybackRate / (time));

  tickingAudio.loop = true;
  tickingAudio.play();

  setInterval(() => {
    playbackRate += playbackRateIncrease;
    tickingAudio.playbackRate = playbackRate;
  }, 1000);

  isTicking.value = true;

  setTimeout(async () => {
    await explosionAudio.play();
    imagePath.value = '/explosion.png';
    endBomb();
  }, 1000 * time);

}

const endBomb = () => {
  tickingAudio.pause();
  tickingAudio.currentTime = 0;
  tickingAudio.playbackRate = initialPlaybackrate;
  isTicking.value = false;
}

</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.activate-button {
  font-size: 24px;
  font-weight: bold;
  font-family: "Bookman Old Style", serif;

  border-radius: 10px;

  outline-offset: 5px;
  outline: white 5px solid;

  margin-top: 20px;

  background: black;
  color: gold;
}
</style>
