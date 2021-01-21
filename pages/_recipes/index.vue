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
		>
			<!-- Display single page content -->
			<div v-if="this.body.length">
				<markdown-content :content="this.body" />
				<footing
					:title="this.title"
					:description="this.description"
					:authorName="this.authorName"
				/>
			</div>

			<!-- Display list of recipes in category -->
			<!-- @see  https://stackoverflow.com/a/61375490/3893936-->
			<!-- <client-only v-else> -->
			<component
				v-for="(recipe, index) in recipeList"
				:is="recipe"
				:key="index"
			/>
			<!-- </client-only> -->
		</main-content>
	</div>
</template>

<script>
import Recipe from '~/components/Recipe'

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
			body: '',
			recipeList: [],
		}
	},

	beforeMount() {
		try {
			let page = require(`~/content/pages/${this.slug}.json`)
			console.log('page', page)

			this.jumbotron = page.jumbotron
			this.authorAvatar = page.authorAvatar
			this.authorName = page.authorName
			this.title = page.title
			this.icon = page.icon
			this.date = page.date
			this.description = page.description
			this.body = page.body
			this.recipeList = []
		} catch (e) {
			try {
				let category = require(`~/content/categories/${this.slug}.json`)
				console.log('category', category)

				this.jumbotron = category.jumbotron
				this.authorAvatar = ''
				this.authorName = ''
				this.title = category.title
				this.icon = category.icon
				this.date = ''
				this.description = category.description
				this.tags = []
				this.body = ''

				let recipes = this.$store.state.recipes[this.slug]

				recipes.forEach((recipe) => {
					let categorySlug = this.slug
					/**
					 * @see https://forum.vuejs.org/t/how-to-add-or-remove-vue-js-component-dynamically-programmatically-or-on-the-fly/32356/3
					 * @tutorial https://jsfiddle.net/jamesbrndwgn/ku7m1dp0/9/
					 */
					const recipeComponent = {
						render(component) {
							return component(Recipe, {
								props: {
									path:
										'/' + categorySlug + '/' + recipe.slug,
									title: recipe.title,
									description: recipe.description,
									tags: recipe.tags,
									icon: recipe.icon,
									jumbotron: recipe.jumbotron,
									date: recipe.date,
								},
							})
						},
					}
					this.recipeList.push(recipeComponent)
				}, this)
			} catch (e) {
				console.log('tag', this.slug)

				this.jumbotron = ''
				this.authorAvatar = ''
				this.authorName = ''
				this.title = '#' + this.slug
				this.icon = ''
				this.date = ''
				this.description = ''
				this.body = ''

				let recipes = this.$store.state.taggedRecipes[this.slug]

				recipes.forEach((recipe) => {
					let categorySlug = recipe.category[0]
					/**
					 * @see https://forum.vuejs.org/t/how-to-add-or-remove-vue-js-component-dynamically-programmatically-or-on-the-fly/32356/3
					 * @tutorial https://jsfiddle.net/jamesbrndwgn/ku7m1dp0/9/
					 */
					const recipeComponent = {
						render(component) {
							return component(Recipe, {
								props: {
									path:
										'/' + categorySlug + '/' + recipe.slug,
									title: recipe.title,
									description: recipe.description,
									tags: recipe.tags,
									icon: recipe.icon,
									jumbotron: recipe.jumbotron,
									date: recipe.date,
								},
							})
						},
					}
					this.recipeList.push(recipeComponent)
				}, this)
			}
		}
	},

	async asyncData({ params }) {
		const slug = params.slug
		return {
			slug,
		}
	},

	head() {
		return {
			title: this.title,
		}
	},
}
</script>
