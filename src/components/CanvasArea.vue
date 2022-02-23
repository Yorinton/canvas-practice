<template>
  <div ref="parent">
    <canvas ref="canvas" id="tutorial" @dblclick.stop="addInput" @mousedown="onMouseDown" @mousemove="onMouseMove" @mouseup="onMouseUp" tabindex="0" @click="onClick"></canvas>
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
  canvas.value.width = window.innerWidth
  canvas.value.height = window.innerHeight
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

const rectWidth = ref(200)
const rectHeight = ref(200)
const rectX = ref(0)
const rectY = ref(0)
const dragging = ref(false)
// clickイベントなどでドラッグ直後のイベントか純粋なクリックイベントか判定する
const ImmediatelyAfterDrag = ref(false)
const relX = ref(0)
const relY = ref(0)
const ctx = ref() as Ref<CanvasRenderingContext2D>
const lastClick = ref({x: 0, y: 0})

const initCanvasContent = () => {
  const full = document.getElementById('tutorial') as HTMLCanvasElement
  ctx.value = full.getContext('2d') as CanvasRenderingContext2D
  ctx.value.fillStyle = "rgb(255,255,0)"

  rectX.value = canvas.value.width / 2 - rectWidth.value / 2
  rectY.value = canvas.value.height / 2 - rectHeight.value / 2

  // 影付きの付箋追加
  ctx.value.shadowColor = "rgb(0,0,0,0.2)"
  ctx.value.shadowOffsetX = 3
  ctx.value.shadowOffsetY = 3
  ctx.value.shadowBlur = 3
  ctx.value.fillRect(rectX.value, rectY.value, rectWidth.value, rectHeight.value)
}

const onMouseDown = (e: MouseEvent) => {
  // キャンバスの左上端の座標を取得
  const offsetX = canvas.value.getBoundingClientRect().left;
  const offsetY = canvas.value.getBoundingClientRect().top;

  // マウスが押された座標を取得
  const x = e.clientX - offsetX;
  const y = e.clientY - offsetY;

  // オブジェクト上の座標かどうかを判定
  if (rectX.value < x && (rectX.value + rectWidth.value) > x && rectY.value < y && (rectY.value + rectHeight.value) > y) {
    dragging.value = true; // ドラッグ開始
    relX.value = rectX.value - x;
    relY.value = rectY.value - y;
  } else {
    dragging.value = false
  }
}

const onMouseMove = (e: MouseEvent) => {
  // キャンバスの左上端の座標を取得
  const offsetX = canvas.value.getBoundingClientRect().left;
  const offsetY = canvas.value.getBoundingClientRect().top;
  
  // マウスが押された座標を取得
  const x = e.clientX - offsetX;
  const y = e.clientY - offsetY;

  if(dragging.value) {
    // 付箋をドラッグ
    const rectXBefore = rectX.value
    const rectYBefore = rectY.value
    rectX.value = x + relX.value
    rectY.value = y + relY.value

    ctx.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
    ctx.value.fillRect(rectX.value, rectY.value, rectWidth.value, rectHeight.value)
    if (ctx.value.strokeStyle !== '#000000') {
      ctx.value.strokeRect(rectX.value, rectY.value, rectWidth.value, rectHeight.value)
    }
    ImmediatelyAfterDrag.value = true
  }
}

// const imgData = ref() as Ref<ImageData>
// const onKeyDown = async (e: KeyboardEvent) => {
//   if (e.metaKey && e.key === 'c') {
//     imgData.value = ctx.value.getImageData(rectX.value, rectY.value, rectWidth.value + 3, rectHeight.value + 3);
//   }
//   if (e.metaKey && e.key === 'v') {
//     if (lastClick.value.x > 0 || lastClick.value.y > 0) {
//       ctx.value.putImageData(imgData.value, lastClick.value.x, lastClick.value.y)
//     } else {
//       ctx.value.putImageData(imgData.value, rectX.value + 20, rectY.value - 20)
//     }
//   }
// }

const onClick = (e: MouseEvent) => {
  // キャンバスの左上端の座標を取得
  const offsetX = canvas.value.getBoundingClientRect().left;
  const offsetY = canvas.value.getBoundingClientRect().top;

  // マウスが押された座標を取得
  lastClick.value.x = e.clientX - offsetX
  lastClick.value.y = e.clientY - offsetY

  // クリックしたら付箋に線をつける
  if (rectX.value < lastClick.value.x && (rectX.value + rectWidth.value) > lastClick.value.x && rectY.value < lastClick.value.y && (rectY.value + rectHeight.value) > lastClick.value.y) {
    if (ctx.value.strokeStyle === '#000000' && !ImmediatelyAfterDrag.value) {
      ctx.value.strokeStyle = "rgba(255,165,0)"
      ctx.value.strokeRect(rectX.value, rectY.value, rectWidth.value, rectHeight.value)
    }
  } else {
    ctx.value.strokeStyle = "#000000"
    ctx.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
    ctx.value.fillRect(rectX.value, rectY.value, rectWidth.value, rectHeight.value)
  }
  ImmediatelyAfterDrag.value = false
}

const onMouseUp = (e: MouseEvent) => {
  dragging.value = false
}

</script>

<style lang="scss" scoped>

</style>
