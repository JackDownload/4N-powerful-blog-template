<template>
	<div class="Recipe">
		<nuxt-link class="RecipeContainer" :to="path" :title="title">
			<div class="RecipeInfoContainer">
				<div class="RecipeTitleContainer">
					<div class="RecipeIcon">
						<nuxt-image
							class="RecipeIconImage"
							:src="iconImagePath"
							alt=" "
						/>
					</div>
					{{ title }}
				</div>

				<div class="RecipeDate">{{ date }}</div>
				<div class="RecipeDescription">
					{{ description }}
				</div>
				<div class="RecipeTags">
					<nuxt-link
						class="RecipeTag"
						v-for="(tag, index) in recipeTags"
						:key="index"
						:to="`/${tag}`"
						:title="tag"
					>
						#{{ tag }}
					</nuxt-link>
				</div>
			</div>

			<div class="RecipeJumbotron">
				<nuxt-image
					class="RecipeJumbotronImage"
					:src="jumbotronImagePath"
					:placeholder="true"
					alt=" "
				/>
			</div>
		</nuxt-link>
	</div>
</template>

<script>
export default {
	props: {
		path: String,
		title: String,
		description: String,
		tags: Array,
		icon: String,
		jumbotron: String,
		date: String,
	},

	computed: {
		iconImagePath: function () {
			return this.resolveImagePath(this.icon)
		},
		jumbotronImagePath: function () {
			return this.resolveImagePath(this.jumbotron)
		},
		recipeTags: function () {
			if (this.tags) {
				return this.tags.filter((tag) => tag != '')
			}
			return []
		},
	},
}
</script>

<style>
.Recipe {
	width: 100%;
	max-width: 100%;
	margin-bottom: 20px;
	display: flex;
}
.Recipe:hover {
	background: var(--bg-hover);
}

.RecipeContainer {
	text-decoration: none;
	min-height: 150px;
	width: 100%;
	display: flex;
	overflow: hidden;
	border: 2px solid var(--bg-hover);
	border-radius: 3px;
	color: inherit;
}

.RecipeInfoContainer {
	flex: 4 1 180px;
	padding: 12px 14px 14px;
	text-align: left;
}

.RecipeIcon {
	height: 40px;
	width: 40px;
	float: left;
	border-radius: 3px;
	margin-right: 5px;
	margin-top: 5px;
}

.RecipeIconImage {
	height: 100%;
	width: 100%;
}

.RecipeDate {
	display: inline-flex;
	align-items: center;
	border-radius: 3px;
	padding-left: 6px;
	padding-right: 6px;
	font-size: 12px;
	color: var(--color-secondary);
	background: var(--bg-secondary);
}

.RecipeDescription {
	font-size: 12px;
	color: var(--color-secondary);
	margin-top: 5px;
}

.RecipeJumbotron {
	flex: 1 1 180px;
}

.RecipeJumbotronImage {
	display: block;
	object-fit: cover;
	border-radius: 1px;
	width: 100%;
	height: 100%;
}

.RecipeTag {
	font-size: 12px;
	color: var(--color-secondary);
	margin: 3px;
}

.Recipe img {
	opacity: 1 !important;
	transition: opacity 800ms ease 0ms !important;
}
</style>
