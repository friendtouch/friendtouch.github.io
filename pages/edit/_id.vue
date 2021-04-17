<template>
	<div>
		<form class="form" @submit.prevent="submit">
			<div class="field">
				<v-text-field filled color="white" label="ID" v-model="form.id" readonly hide-details></v-text-field>
			</div>
			<div class="field mt-4">
				<v-text-field filled color="white" label="Password" v-model="form.pw" hide-details></v-text-field>
			</div>
			<n-title>Links</n-title>
			<div class="field px-2" v-for="(link,i) of form.data.sp" :key="i" v-if="settings.length">
				<v-menu top close-on-click>
					<template v-slot:activator="{ on, attrs }">
						<v-btn color="white" dark v-bind="attrs" v-on="on" icon>
							<v-icon>fab fa-{{settings.filter(x=>x.id == form.data.sp[i].id)[0].icon}}</v-icon>
						</v-btn>
					</template>

					<v-list>
						<v-list-item v-for="item of settings" :key="item.id" @click="form.data.sp[i].id=item.id">
							<v-list-item-title>
								<v-icon>fab fa-{{item.icon}}</v-icon>
								{{item.display_name}}
							</v-list-item-title>
						</v-list-item>
					</v-list>
				</v-menu>
				<v-text-field
					type="url"
					class="edit-input"
					required
					placeholder="https://"
					v-model="form.data.sp[i].url"
					hide-details
					filled
					rounded
				/>

				<v-btn icon color="white" @click="form.data.sp=form.data.sp.filter((x,j)=>i!=j)">
					<v-icon>fas fa-times</v-icon>
				</v-btn>
			</div>
			<div class="field">
				<v-btn text color="white" block @click="addLink">
					<v-icon>fas fa-plus</v-icon>
				</v-btn>
			</div>
			<button class="btn btn-submit neon-text neon-shadow" type="submit">Submit</button>
		</form>
	</div>
</template>

<script> 

export default {
	async created() {
		this.form.id = this.$route.params.id
		this.form.data = await fetch(`https://friend-touch.com/api/getdata/${this.$route.params.id}`).then(x => x.json())
		this.settings = await fetch(`https://friend-touch.com/api/settings`).then(x => x.json())
	},
	data() {
		return {
			form: {
				id: "",
				data: {
					sp: [{
						url: "", id: "ig"
					}],
				},
				pw: "",
			},
			settings: [],
		}
	},
	methods: {
		addLink() {
			this.form.data.sp.push({
				url: "", id: "ig"
			})
		},
		async submit() {
			let result = await fetch('https://friend-touch.com/api/change', {
				body: JSON.stringify(this.form),
				headers: {
					'content-type': 'application/json'
				},
				method: 'POST',
				//mode: 'cors',
			}).then(x => x.json())
			console.log(result.text)
			if (result.text == "OK") {
				this.$router.push(`/profile/${this.$route.params.id}`)
			} else {
				alert(result.text)
			}
		}
	}
}
</script>
