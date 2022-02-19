<template>
  <div>
    <canvas id="tutorial" width="1500" height="1500"></canvas>
  </div>
</template>
<script setup lang="ts">
import { onMounted } from 'vue'


onMounted(() => {
  const full = document.getElementById('tutorial') as HTMLCanvasElement
  const ctx = full.getContext('2d') as CanvasRenderingContext2D
  ctx.fillStyle = "rgb(200,0,0)"
  ctx.fillRect(10, 10, 50, 50)
  ctx.fillStyle = "rgba(0, 0, 200, 0.5)"
  ctx.fillRect(30, 30, 50, 50)
  ctx.clearRect(35, 35, 30, 30)
  ctx.strokeRect(37, 37, 25, 25)

  ctx.fillStyle = "rgba(200,0,0, 0.5)"
  ctx.strokeStyle = "rgba(1, 155, 155)"
  ctx.beginPath()
  ctx.moveTo(75, 50)
  ctx.lineTo(100, 75)
  ctx.lineTo(100, 25)
  ctx.closePath()
  ctx.stroke()
  ctx.fill()
})


</script>

<style lang="scss" scoped>

</style>
