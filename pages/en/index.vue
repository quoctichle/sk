<template>
	<div class="page-wrap">
		<div class="overlay"></div>

		<div class="lang-top">
			<a class="lang-btn inactive" href="/vi">Tiếng Việt</a>
			<button class="lang-btn active" disabled>English</button>
		</div>

		<main class="page-main">
			<h1 style="color:#1f5a22; text-shadow: 0 6px 18px rgba(0,0,0,0.28);">Guess the Number - Win Gold This Year</h1>

			<div class="form-box">
				<form @submit.prevent="submitForm">
					<div class="form-row">
						<label class="form-label">Facebook:</label>
						<input class="form-input" type="text" placeholder="Enter your Facebook name" v-model="facebook">
					</div>

					<div class="form-row">
						<label class="form-label">Customer ID:</label>
						<input class="form-input" type="text" placeholder="Enter customer ID" v-model="customerId">
					</div>

					<div class="form-row">
						<label class="form-label">Email:</label>
						<input class="form-input" type="email" placeholder="Enter your email" v-model="email">
					</div>

					<div class="form-row">
						<label class="form-label">Prize Code:</label>
						<input class="form-input" type="text" placeholder="Enter prize code" v-model="prizeCode">
					</div>

					<div class="submit-wrap">
						<button class="submit-btn" type="submit">CONTINUE</button>
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
	router.push('/en/result')
}
</script>

*** End Patch
