<template>
  <div ref="parent">
    <canvas ref="canvas" id="tutorial" @click.stop="putRect" @dblclick.stop="addInput"></canvas>
  </div>
</template>
<script setup lang="ts">
import { onMounted, Ref, ref } from 'vue'

/**
 * 画面全体のcanvasを生成
 */
const parent = ref() as Ref<HTMLDivElement>
const canvas = ref() as Ref<HTMLCanvasElement>
window.addEventListener('resize', () => {
  resizeCanvas()
  initCanvasContent()
})

const resizeCanvas = () => {
  canvas.value.style.width = window.innerWidth + 'px'
  canvas.value.style.height = window.innerHeight + 'px'
  canvas.value.width = window.innerWidth * window.devicePixelRatio
  canvas.value.height = window.innerHeight * window.devicePixelRatio
}

const putRect = (e: MouseEvent) => {
  const full = document.getElementById('tutorial') as HTMLCanvasElement
  const ctx = full.getContext('2d') as CanvasRenderingContext2D
  ctx.fillStyle = "rgb(255,255,0)"
  ctx.fillRect(e.clientX, e.clientY, 200, 200)
}

const addInput = (e: MouseEvent) => {
  const input = document.createElement('input')
  input.id = 'input'
  input.style.top = e.clientY + 'px'
  input.style.left = e.clientX + 'px'
  input.style.position = 'absolute'
  document.body.appendChild(input)
}


onMounted(() => {
  resizeCanvas()
  initCanvasContent()
})

const initCanvasContent = () => {
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

  ctx.beginPath()
  ctx.arc(100, 100, 50, 0, Math.PI * 2, true)
  ctx.moveTo(135, 100) // 始点を移動する
  // 中心の位置(x, y)、半径(radius)、start/endAngle(開始/終了点のx軸からの正方向のラジアン角度), anticlockwise(反時計回りか)
  // radians = (Math.PI/180)*degrees
  ctx.arc(100, 100, 35, 0, Math.PI, false)
  ctx.moveTo(90, 90)
  ctx.arc(85, 90, 5, 0, Math.PI * 2, true)
  ctx.moveTo(120, 90)
  ctx.arc(115, 90, 5, 0, Math.PI * 2, true)
  ctx.stroke()
}

</script>

<style lang="scss" scoped>

</style>
