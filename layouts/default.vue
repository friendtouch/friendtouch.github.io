<template>
	<v-app dark style="background: transparent">
		<div class="neon-title neon-text neon-shadow">Friend Touch</div>
		<nuxt />
		<v-divider />
		<footer class="mt-1 d-flex px-2 align-center">
			<div class="flex-grow-1">Designed in Taiwan</div>
			<div>
				<v-btn icon color="white" href="https://www.instagram.com/friend.touch/" target="_blank">
					<v-icon>fab fa-instagram</v-icon>
				</v-btn>
				<v-btn icon color="white" href="https://www.facebook.com/Friend.Touch.Taiwan/" target="_blank">
					<v-icon>fab fa-facebook</v-icon>
				</v-btn>
			</div>
		</footer>
	</v-app>
</template>
<style lang="sass">
footer
	opacity: .7
	font-size: 14px
	text-align: left
</style>
<script>
export default {
	data() {
		return {
		}
	}
}
</script>
