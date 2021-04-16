<template>
	<div>
		<v-list subheader style="background: transparent">
			<v-list-item v-for="link in links" :key="link.value" :to="link.value">
				<v-list-item-avatar>
					<v-icon dark v-text="link.icon"></v-icon>
				</v-list-item-avatar>

				<v-list-item-content>
					<v-list-item-title v-text="link.value"></v-list-item-title>
				</v-list-item-content>
			</v-list-item>
		</v-list>
		<v-btn color="primary" fab large dark bottom right fixed :to="'/edit/'+$route.params.id">
			<v-icon class="material-icons-outlined">edit</v-icon>
		</v-btn>
	</div>
</template>

<script> 

export default {
	created() {
		this.links = await fetch(`https://friend-touch.com/api/getdata/${this.$route.params.id}`).then(x => x.json)
	},
	data() {
		return {
			links: []
		}
	},
	methods: {

	}
}
</script>
