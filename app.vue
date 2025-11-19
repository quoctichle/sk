<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtPage />
    <ClientOnly>
      <Snowfall />
    </ClientOnly>
    <ClientOnly>
      <div id="bg-music" aria-hidden="false">
        <audio ref="bgAudio" src="/music.mp3" loop preload="auto" autoplay playsinline></audio>
        <button class="music-toggle" @click="toggleMusic" aria-label="Toggle background music">
          <span v-if="isPlaying">🔊</span>
          <span v-else>🔈</span>
        </button>

        <div v-if="needGesture" class="audio-gesture" @click="handleGesture">
          <div class="gesture-inner">
            <div class="gesture-text">Chạm để bật âm thanh</div>
            <div class="gesture-sub">Tap to enable sound</div>
          </div>
        </div>
      </div>
    </ClientOnly>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import Snowfall from '~/components/Snowfall.vue'

const bgAudio = ref(null)
const isPlaying = ref(false)
const needGesture = ref(false)

const playAudio = async () => {
  if (!bgAudio.value) return
  try {
    // Try to play unmuted first (best effort); browsers may still block this.
    bgAudio.value.muted = false
    bgAudio.value.autoplay = true
    await bgAudio.value.play()
    isPlaying.value = !bgAudio.value.paused
    } catch (e) {
      // Audible autoplay blocked — try to play muted as a fallback so audio element is active
      try {
        bgAudio.value.muted = true
        await bgAudio.value.play()
        // mark playing but currently muted
        isPlaying.value = true
      } catch (err) {
        // both audible and muted autoplay blocked -> require user gesture
        isPlaying.value = false
        needGesture.value = true
      }
  }
}

const toggleMusic = async () => {
  if (!bgAudio.value) return
  if (isPlaying.value) {
    bgAudio.value.pause()
    isPlaying.value = false
  } else {
    bgAudio.value.muted = false
    await playAudio()
  }
}

const onFirstGesture = () => {
  if (!bgAudio.value) return
  if (bgAudio.value.muted) {
    bgAudio.value.muted = false
    bgAudio.value.play().catch(()=>{})
    isPlaying.value = !bgAudio.value.paused
  }
  needGesture.value = false
}

const handleGesture = async () => {
  // Called when the full-screen gesture overlay is tapped
  if (!bgAudio.value) return
  try {
    bgAudio.value.muted = false
    await bgAudio.value.play()
    isPlaying.value = true
  } catch (e) {
    // if still fails, keep muted but mark as playing so element is active
    try { await bgAudio.value.play(); isPlaying.value = true } catch {}
  }
  needGesture.value = false
}

onMounted(() => {
  // Try to start playback on mount. If the browser blocks audible autoplay,
  // the fallback will play muted and we still listen for the first user gesture
  // to unmute.
  if (bgAudio.value) {
    // attempt to play audible immediately (best-effort)
    playAudio()
  }
  document.addEventListener('click', onFirstGesture, { once: true })
  document.addEventListener('pointerdown', onFirstGesture, { once: true, passive: true })
  document.addEventListener('touchstart', onFirstGesture, { once: true, passive: true })
  // expose a global helper so pages can start/unmute audio on user actions
  // pages should call `window.startBgAudio()` when the user interacts (e.g., Continue button)
  window.startBgAudio = async () => {
    try {
      if (!bgAudio.value) return
      bgAudio.value.muted = false
      await bgAudio.value.play()
      isPlaying.value = true
      needGesture.value = false
    } catch (e) {
      // fallback: try to play muted so audio element is active
      try { await bgAudio.value.play(); isPlaying.value = true; } catch {}
    }
  }
})

onBeforeUnmount(() => {
  document.removeEventListener('click', onFirstGesture)
  document.removeEventListener('pointerdown', onFirstGesture)
  document.removeEventListener('touchstart', onFirstGesture)
})
</script>

<style>
@font-face {
  font-family: 'FS Magistral';
  src: url('/fonts/FS_MAGISTRAL-BOLD.ttf') format('truetype');
  font-weight: bold;
  font-style: normal;
}

h1, h2, h3, h4, h5, h6, button, a {
  font-family: 'FS Magistral', sans-serif;
}
</style>

<style>
/* Layout classes and responsive rules */
.page-wrap {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  background: url('/background.jpg') center/cover no-repeat;
}
.page-wrap .overlay {
  position: absolute; inset: 0; background: rgba(0,0,0,0.25); z-index: 0;
}
.lang-top {
  position: absolute; top: 24px; left: 50%; transform: translateX(-50%); z-index: 20; display:flex; gap:12px;
}
.lang-btn { border-radius: 22px; padding: 10px 26px; font-weight: 700; font-size: 0.95rem; display:inline-block; }
.lang-btn.active { background: linear-gradient(135deg,#52a94b 0%,#7fb069 100%); color: #fff; border: none; }
.lang-btn.inactive { background: #fff; color:#333; border:2px solid rgba(0,0,0,0.08); }
.page-main { text-align:center; max-width:680px; width:100%; padding: 20px; position: relative; z-index:10; }
.form-box { background: linear-gradient(135deg,#2f7f3a 0%, #7fb069 100%); padding:36px; border-radius:18px; box-shadow:0 10px 30px rgba(0,0,0,0.18); }
.form-row { background:#fff; padding:14px; border-radius:8px; display:flex; align-items:center; gap:12px; margin-bottom:18px; }
.form-label { min-width:140px; font-weight:700; color:#2d5016; text-align:left; display:inline-block; }
.form-input { flex:1; border:none; outline:none; background:transparent; font-size:1rem; }
.submit-wrap { text-align:center; margin-top:6px; }
.submit-btn { background:#c0c0c0; color:#333; border:none; padding:12px 36px; font-size:1.05rem; font-weight:700; border-radius:8px; }

/* Responsive tweaks */
@media (max-width: 600px) {
  .lang-top { top: 12px; gap:10px; }
  .lang-btn { padding: 8px 18px; font-size:0.9rem; }
  .page-main { padding: 12px; }
  .form-box { padding:20px; border-radius:14px; }
  .form-label { min-width:110px; font-size:0.95rem; text-align:left; }
  .form-row { padding:12px; }
  .submit-btn { padding:10px 28px; }
  .page-main h1 { font-size:1.6rem; }
}

@media (max-width: 400px) {
  .form-label { min-width:90px; font-size:0.9rem; text-align:left; }
  .lang-top { top:8px; }
  .page-main h1 { font-size:1.4rem; }
}

/* Falling gold (coins/ingots) animation */
.falling-coins { position:absolute; inset:0; pointer-events:none; overflow:hidden; z-index:9999; }
.falling-coins .coin { position:absolute; top:-10%; width:44px; opacity:0; transform:translateY(-10vh); background: url('/gold.png') center/contain no-repeat, radial-gradient(circle at 35% 30%, #ffd54a 0%, #f0b000 45%, #c68a00 100%); background-size: contain, auto; border-radius:50%; box-shadow:0 4px 8px rgba(0,0,0,0.12); }
@keyframes fall {
  0% {
    transform: translateY(-10vh) translateX(0) rotate(0deg);
    opacity: 0;
  }
  8% {
    opacity: 1;
  }
  25% {
    transform: translateY(20vh) translateX(12px) rotate(120deg);
  }
  50% {
    transform: translateY(50vh) translateX(-12px) rotate(360deg);
  }
  75% {
    transform: translateY(80vh) translateX(8px) rotate(540deg);
  }
  100% {
    transform: translateY(110vh) translateX(0) rotate(720deg);
    opacity: 1;
  }
}

/* Per-coin positions, sizes, durations and delays for variety */
.falling-coins .coin:nth-child(1)  { left:4%;  --d:6.8s;  animation-delay:0.2s;  width:36px; --s:3s; }
.falling-coins .coin:nth-child(2)  { left:12%; --d:8.1s;  animation-delay:1.0s;  width:30px; --s:3.8s; }
.falling-coins .coin:nth-child(3)  { left:22%; --d:7.0s;  animation-delay:0.6s;  width:40px; --s:2.6s; }
.falling-coins .coin:nth-child(4)  { left:30%; --d:9.0s;  animation-delay:1.6s;  width:34px; --s:3.2s; }
.falling-coins .coin:nth-child(5)  { left:38%; --d:6.2s;  animation-delay:0.4s;  width:38px; --s:2.9s; }
.falling-coins .coin:nth-child(6)  { left:50%; --d:7.6s;  animation-delay:1.2s;  width:42px; --s:3.6s; }
.falling-coins .coin:nth-child(7)  { left:60%; --d:8.8s;  animation-delay:0.8s;  width:30px; --s:2.7s; }
.falling-coins .coin:nth-child(8)  { left:68%; --d:6.6s;  animation-delay:1.4s;  width:36px; --s:3.1s; }
.falling-coins .coin:nth-child(9)  { left:76%; --d:7.9s;  animation-delay:0.3s;  width:34px; --s:2.8s; }
.falling-coins .coin:nth-child(10) { left:84%; --d:9.4s;  animation-delay:1.8s;  width:38px; --s:3.9s; }
.falling-coins .coin:nth-child(11) { left:88%; --d:6.5s;  animation-delay:0.9s;  width:32px; --s:3.0s; }
.falling-coins .coin:nth-child(12) { left:95%; --d:8.3s;  animation-delay:0.5s;  width:28px; --s:2.5s; }

/* Add a slight horizontal sway to each coin for a natural fall */
.falling-coins .coin { animation-name: fall; animation-duration: var(--d,8s); animation-timing-function: linear; animation-iteration-count: infinite; }
@keyframes sway {
  0% { transform: translateY(var(--y,0)) translateX(0) rotate(0deg); }
  50% { transform: translateY(var(--y,0)) translateX(18px) rotate(180deg); }
  100% { transform: translateY(var(--y,0)) translateX(0) rotate(360deg); }
}

@media (max-width: 600px) {
  .falling-coins .coin { display:block; }
  .falling-coins .coin { width: calc(40px * 0.9); }
}

/* Background music control */
#bg-music { position: fixed; right: 16px; bottom: 16px; z-index: 2000; }
.music-toggle { background: rgba(255,255,255,0.9); border: none; padding:8px 10px; border-radius:10px; box-shadow:0 6px 18px rgba(0,0,0,0.18); cursor:pointer; font-size:18px; }
.music-toggle:focus { outline:2px solid rgba(0,0,0,0.12) }

@media (max-width: 600px) {
  #bg-music { right: 10px; bottom: 10px }
  .music-toggle { padding:6px 8px; font-size:16px }
}
</style>
