<template>
  <div class="page-wrap">
    <div class="overlay"></div>

    <main class="page-main">
      <h1 style="color:#1f5a22; text-shadow: 0 6px 18px rgba(0,0,0,0.28);">Good luck!</h1>

      <div class="result-box">
        <a href="https://www.facebook.com/share/r/17QKGm9UjY/" target="_blank" rel="noopener noreferrer">
          <img class="promo-img" src="/promo.jpg" alt="Promo image" />
        </a>
      </div>

      <div class="instruction">
        <p>After submitting the form, the customer visits the post:<br>
        “Guess the Number - Win Gold This Year”</p>

        <p>There, the player must:</p>
        <ul>
          <li>Comment a <strong>New Year wish</strong> addressed to <strong>SUNSHINE</strong></li>
        </ul>

        <p>Example: <em>“Wishing SUNSHINE a brilliant New Year — connecting countless good luck!”</em></p>
      </div>
    </main>
  </div>
</template>

<script setup>
// nothing needed here
</script>

<style scoped>
.result-box {
  display:flex;
  justify-content:center;
  align-items:center;
  margin-top:20px;
}
.promo-img{
  max-width:90%;
  width:520px;
  border-radius:12px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.35);
  cursor:pointer;
}
@media(max-width:480px){
  .promo-img{ width:320px }
}

.instruction{
  margin:18px auto 36px;
  max-width:760px;
  color:#13421a;
  font-size:16px;
  line-height:1.45;
  background: rgba(255,255,255,0.02);
  padding:8px 14px;
  border-radius:10px;
  text-align:left;
}
.instruction p{ margin:6px 0 }
.instruction ul{ margin:6px 0 0 20px }
.instruction li{ margin:6px 0 }
</style>
