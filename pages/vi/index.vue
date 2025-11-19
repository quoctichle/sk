<template>
	<div class="page-wrap">
		<div class="overlay"></div>

		<div class="lang-top">
			<button class="lang-btn active" disabled>Tiếng Việt</button>
			<a class="lang-btn inactive" href="/en">English</a>
		</div>

		<main class="page-main">
			<h1 style="color:#1f5a22; text-shadow: 0 6px 18px rgba(0,0,0,0.28);">Đoán số dễ dàng - Rinh vàng đầu năm</h1>

			<div class="form-box">
				<form @submit.prevent="submitForm">
					<div class="form-row">
						<label class="form-label">Tên Facebook:</label>
						<input class="form-input" type="text" placeholder="Nhập tên Facebook của bạn" v-model="facebook">
					</div>

					<div class="form-row">
						<label class="form-label">ID khách hàng:</label>
						<input class="form-input" type="text" placeholder="Nhập ID khách hàng" v-model="customerId">
					</div>

					<div class="form-row">
						<label class="form-label">Email:</label>
						<input class="form-input" type="email" placeholder="Nhập email của bạn" v-model="email">
					</div>

					<div class="form-row">
						<label class="form-label">Mã quay thưởng:</label>
						<input class="form-input" type="text" placeholder="Nhập mã quay thưởng" v-model="prizeCode">
					</div>

					<div class="submit-wrap">
						<button class="submit-btn" type="submit">TIẾP TỤC</button>
					</div>
				</form>
			</div>
		</main>
	</div>
</template>

<script setup>
const facebook = ref('')
const customerId = ref('')
const email = ref('')
const prizeCode = ref('')

const router = useRouter()

const submitForm = async () => {
	// Start background audio on this user gesture (if browser blocked autoplay)
	try { if (window?.startBgAudio) await window.startBgAudio() } catch(e){}
	// Navigate to result page where the promo image is shown
	router.push('/vi/result')
}
</script>
*** End Patch
