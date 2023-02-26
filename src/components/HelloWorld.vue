<template>
  <div ref="lottieRef" class="ani-box"></div>
  <!-- <canvas id="canvas" width="500" height="500" ref="svgaRef" class="ani-box"></canvas> -->
  <div id="svgaRef" ref="svgaRef" class="ani-box"></div>
  <canvas class="ani-box" id="pag"></canvas>
  <div class="video-box">
    <video class="video" src="../assets/fire1.mp4" autoplay muted loop></video>
  </div>
  <img src="../assets/chassis3.png" />
  <img class="diff-pic" src="../assets/chassis3.png" />
</template>
<script setup>
import { onMounted, ref } from 'vue'
import lottie from 'lottie-web'
import redJson from '../assets/red.json'
import SVGA from 'svgaplayerweb'
// import { Parser, Player } from 'svga'
// import redSvga from '../assets/red.svga'
import { PAGInit } from 'libpag';
const lottieRef = ref()
const svgaRef = ref()
onMounted(() => {
  const lottieAnimation = lottie.loadAnimation({
    container: lottieRef.value,
    renderer: 'svg',
    loop: true,
    autoplay: true,
    animationData: redJson
  })
  lottieAnimation.play()


  const player = new SVGA.Player('#svgaRef')
  const parser = new SVGA.Parser('#svgaRef')
  parser.load('/red.svga', function (videoItem) {
    player.setVideoItem(videoItem)
    player.startAnimation()
  })





  PAGInit().then((PAG) => {
  const url = '/red.pag';
  fetch(url)
    .then((response) => response.blob())
    .then(async (blob) => {
      const file = new window.File([blob], url.replace(/(.*\/)*([^.]+)/i, '$2'));
      const pagFile = await PAG.PAGFile.load(file);
      // document.getElementById('pag').width = pagFile.width();
      // document.getElementById('pag').height = pagFile.height();
      const pagView = await PAG.PAGView.init(pagFile, '#pag');
      pagView.setRepeatCount(0);
      await pagView.play();
    });
});


})
</script>
<style scoped>
.ani-box {
  width: 500px;
  height: 500px;
}
.video-box {
  width: 800px;
  height: 800px;
  background: #0a12ff;
}
.video {
  /* mix-blend-mode: screen; */
}
.diff-pic {
  filter: hue-rotate(329deg) saturate(100%) brightness(100%);
}
</style>
