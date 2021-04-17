<template>
	<div>
		<v-list subheader style="background: transparent;text-align: left;" two-line>
			<v-list-item v-for="item in sp" :key="item.url" :href="item.url" target="_blank">
				<v-list-item-avatar>
					<v-icon dark>fab fa-{{settings.filter(x=>x.id == item.id)[0].icon}}</v-icon>
				</v-list-item-avatar>

				<v-list-item-content>
					<v-list-item-title>{{settings.filter(x=>x.id == item.id)[0].display_name}}</v-list-item-title>
					<v-list-item-subtitle>{{item.url}}</v-list-item-subtitle>
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
	async created() {
		this.settings = await fetch(`https://friend-touch.com/api/settings`).then(x => x.json())
		let result = await fetch(`https://friend-touch.com/api/getdata/${this.$route.params.id}`).then(x => x.json())
		if (result.text) {
			alert(result.text)
			this.$router.push('/')
		}
		this.sp = result.sp
	},
	data() {
		return {
			sp: [], settings: []
		}
	},
	methods: {

	}
}
</script>
