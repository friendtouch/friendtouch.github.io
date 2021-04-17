<template>
	<div>
		<form class="form" @submit.prevent="submit">
			<div class="field">
				<label class="neon-text">ID</label>
				<input type="text" required v-model="form.id" readonly />
			</div>
			<div class="field">
				<label class="neon-text">Password</label>
				<input type="text" required v-model="form.pw" />
			</div>
			<n-title>Links</n-title>
			<div class="field links" v-for="(link,i) of form.links" :key="i">
				<label class="neon-text">{{i+1}}</label>
				<v-select
					:items="icons"
					label="Icon"
					style="max-width: 80px;padding-top: 0;"
					v-model="form.links[i].icon"
					hide-details
				>
					<template slot="selection" slot-scope="data">
						<v-icon>{{data.item}}</v-icon>
					</template>
					<template slot="item" slot-scope="data">
						<v-icon>{{data.item}}</v-icon>
					</template>
				</v-select>
				<input type="url" required placeholder="https://" v-model="form.links[i].value" />
				<v-btn icon color="white" @click="form.links=form.links.filter((x,j)=>i!=j)">
					<v-icon>far fa-trash-alt</v-icon>
				</v-btn>
			</div>
			<div class="field">
				<v-btn text color="white" block @click="addLink">add</v-btn>
			</div>
			<button class="btn btn-submit neon-text neon-shadow" @click="submit">Submit</button>
		</form>
	</div>
</template>

<script> 

export default {
	async created() {
		this.form.id = this.$route.params.id
		this.form.links = await fetch(`https://friend-touch.com/api/getdata/${this.$route.params.id}`).then(x => x.json)
	},
	data() {
		return {
			form: {
				id: "",
				links: [{
					value: "", icon: "fab fa-instagram"
				}],
				pw: "",
			},
			icons: [
				'fab fa-instagram'
			]
		}
	},
	methods: {
		addLink() {
			this.form.links.push({
				value: "", icon: "fab fa-instagram"
			})
		},
		async submit() {
			let result = await fetch('https://friend-touch.com/api/change', {
				body: JSON.stringify(this.form),
				headers: {
					'content-type': 'application/json'
				},
				method: 'POST',
				mode: 'cors',
			}).then(x => x.json())
			if (result.text == "OK") {
				this.$router.push(`/profile/${this.$route.params.id}`)
			} else {
				alert(result.text)
			}
		}
	}
}
</script>
