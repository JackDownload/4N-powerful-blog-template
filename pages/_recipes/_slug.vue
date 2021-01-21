<template>
	<div>
		<side-bar />
		<top-bar />
		<main-content
			:jumbotron="this.jumbotron"
			:authorAvatar="this.authorAvatar"
			:authorName="this.authorName"
			:title="this.title"
			:icon="this.icon"
			:date="this.date"
			:description="this.description"
			:tags="this.tags"
		>
			<!-- Display recipe content -->
			<markdown-content :content="this.body" />
			<footing
				:title="this.title"
				:description="this.description"
				:authorName="this.authorName"
			/>
		</main-content>
	</div>
</template>

<script>
export default {
	data() {
		return {
			jumbotron: '',
			authorAvatar: '',
			authorName: '',
			title: '',
			icon: '',
			date: '',
			description: '',
			tags: [],
			body: '',
			recipe: '',
		}
	},

	beforeMount() {
		try {
			if (!this.recipe) {
				this.recipe = require(`~/content/recipes/${this.slug}.json`)
			}
			let recipe = this.recipe
			console.log('recipe', recipe)

			this.jumbotron = recipe.jumbotron
			this.authorAvatar = recipe.authorAvatar
			this.authorName = recipe.authorName
			this.title = recipe.title
			this.icon = recipe.icon
			this.date = recipe.date
			this.description = recipe.description
			this.tags = recipe.tags
			this.body = recipe.body
		} catch (e) {
			this.body = 'Something wrong: ' + e.message
		}
	},
	async asyncData({ params, payload }) {
		const category = params.category
		const slug = params.slug

		return { category, slug }
		if (payload) return { category, slug, recipe: payload }
		else
			return {
				category,
				slug,
				recipe: await require(`~/content/recipes/${slug}.json`),
			}
	},

	head() {
		return {
			title: this.title,
		}
	},
}
</script>
