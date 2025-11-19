<template>
  <div class="page-wrap">
    <div class="overlay"></div>

    <main class="page-main">
      <h1 style="color:#1f5a22; text-shadow: 0 6px 18px rgba(0,0,0,0.28);">Chúc bạn may mắn !</h1>

      <div class="result-box">
        <a href="https://www.facebook.com/share/r/17QKGm9UjY/" target="_blank" rel="noopener noreferrer">
          <img class="promo-img" src="/promo.jpg" alt="Promo image" />
        </a>
      </div>

      <div class="instruction">
        <p>Sau khi điền xong form, khách hàng truy cập bài viết:<br>
        “Đoán số dễ dàng – Rinh vàng đầu năm”</p>

        <p>Tại đây, người chơi cần:</p>
        <ul>
          <li>Bình luận <strong>Lời chúc năm mới</strong> gửi đến <strong>SUNSHINE</strong></li>
        </ul>

        <p>Ví dụ: <em>“Chúc SUNSHINE năm mới rực rỡ – kết nối vạn điều may!”</em></p>
      </div>
    </main>
  </div>
</template>

<script setup>
// simple page — no script needed for now
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
