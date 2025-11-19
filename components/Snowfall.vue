<template>
  <div id="snowflakeContainer" ref="container">
    <p class="snowflake">❄</p>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue'

const container = ref<HTMLElement | null>(null)

let requestAnimationFrameFn: any = null
let transformProperty: string | null = null
let snowflakes: any[] = []
let browserWidth = 0
let browserHeight = 0
let numberOfSnowflakes = 50
let resetPosition = false
let rafId: number | null = null

function getSupportedPropertyName(props: string[]) {
  for (let i = 0; i < props.length; i++) {
    const p = props[i]
    if (typeof (document.body.style as any)[p] !== 'undefined') return p
  }
  return null
}

class Snowflake {
  element: HTMLElement
  radius: number
  speed: number
  xPos: number
  yPos: number
  counter: number
  sign: number
  constructor(element: HTMLElement, radius: number, speed: number, x: number, y: number) {
    this.element = element
    this.radius = radius
    this.speed = speed
    this.xPos = x
    this.yPos = y
    this.counter = 0
    this.sign = Math.random() < 0.5 ? 1 : -1
    this.element.style.opacity = String(0.5 + Math.random())
    this.element.style.fontSize = `${4 + Math.random() * 30}px`
  }
  update() {
    this.counter += this.speed / 5000
    this.xPos += this.sign * this.speed * Math.cos(this.counter) / 40
    this.yPos += Math.sin(this.counter) / 40 + this.speed / 30
    setTranslate3DTransform(this.element, Math.round(this.xPos), Math.round(this.yPos))
    if (this.yPos > browserHeight) {
      this.yPos = -50
    }
  }
}

function setTranslate3DTransform(el: HTMLElement, x: number, y: number) {
  const d = `translate3d(${x}px, ${y}px, 0)`
  if (transformProperty) el.style[transformProperty as any] = d
}

function getPosition(offset: number, size: number) {
  return Math.round(-1 * offset + Math.random() * (size + 2 * offset))
}

function generateSnowflakes() {
  const sample = container.value?.querySelector('.snowflake') as HTMLElement | null
  if (!sample || !container.value) return
  const parent = sample.parentNode as HTMLElement
  browserWidth = document.documentElement.clientWidth
  browserHeight = document.documentElement.clientHeight
  for (let i = 0; i < numberOfSnowflakes; i++) {
    const node = sample.cloneNode(true) as HTMLElement
    parent.appendChild(node)
    const x = getPosition(50, browserWidth)
    const y = getPosition(50, browserHeight)
    const radius = 5 + Math.random() * 40
    const speed = 4 + Math.random() * 10
    const sf = new Snowflake(node, radius, speed, x, y)
    snowflakes.push(sf)
  }
  parent.removeChild(sample)
  moveSnowflakes()
}

function moveSnowflakes() {
  for (let i = 0; i < snowflakes.length; i++) {
    snowflakes[i].update()
  }
  if (resetPosition) {
    browserWidth = document.documentElement.clientWidth
    browserHeight = document.documentElement.clientHeight
    for (let i = 0; i < snowflakes.length; i++) {
      snowflakes[i].xPos = getPosition(50, browserWidth)
      snowflakes[i].yPos = getPosition(50, browserHeight)
    }
    resetPosition = false
  }
  rafId = requestAnimationFrameFn(moveSnowflakes)
}

function setResetFlag() {
  resetPosition = true
}

onMounted(() => {
  if (typeof window === 'undefined') return
  requestAnimationFrameFn = window.requestAnimationFrame || window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame
  const transforms = ['transform','msTransform','webkitTransform','mozTransform','oTransform']
  transformProperty = getSupportedPropertyName(transforms)
  generateSnowflakes()
  window.addEventListener('resize', setResetFlag)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', setResetFlag)
  if (rafId != null) cancelAnimationFrame(rafId)
  snowflakes = []
})
</script>

<style scoped>
#snowflakeContainer{position:absolute;left:0;top:0}
.snowflake{padding-left:15px;font-size:14px;line-height:24px;position:fixed;color:#ebebeb;user-select:none;z-index:1000;-moz-user-select:none;-ms-user-select:none;-khtml-user-select:none;-webkit-user-select:none;-webkit-touch-callout:none}
.snowflake:hover{cursor:default}
</style>
