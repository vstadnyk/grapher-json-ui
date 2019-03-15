<template>
	<main>
		<h1>{{ title }}</h1>
		<section>
			<div>
				<fieldset>
					<legend>Props</legend>
					<p>
						<label>editable: <input type="checkbox" v-model="editable"/></label>
						<label>brackets: <input type="checkbox" v-model="brackets"/></label>
						<label>comma: <input type="checkbox" v-model="comma"/></label>
					</p>
				</fieldset>
				<grapher
					v-model="data"
					v-bind="{ editable, brackets, comma }"
					class="grapher"
				/>
			</div>
			<fieldset>
				<legend>Preview</legend>
				<textarea
					:value="JSON.stringify(data, null, '    ')"
					disabled
					:class="updated ? 'updated' : null"
				/>
			</fieldset>
		</section>
	</main>
</template>

<script>
import 'grapher-json-ui/dist/index.css'

import Grapher from 'grapher-json-ui'

export default {
	components: { Grapher },
	data: () => ({
		editable: true,
		brackets: true,
		comma: true,
		updated: false,
		title: window.document.title
			.split('')
			.map((w, i) => (!i ? w.toUpperCase() : w))
			.join(''),
		data: {
			isActive: true,
			picture: 'http://placehold.it/32x32',
			age: 23,
			name: 'Naomi Hubbard',
			company: null,
			email: 'naomihubbard@cubicide.com',
			about:
				'Nulla esse eiusmod sint nostrud pariatur occaecat aliqua minim excepteur tempor sint ut officia est. Aliqua sint nostrud et proident elit laboris laborum excepteur do nisi. Minim reprehenderit ea do incididunt in fugiat anim magna anim aliqua velit eu. Sint mollit commodo nulla labore. Duis consectetur fugiat ipsum magna elit ad Lorem sunt.',
			latitude: -8.676648,
			longitude: 109.326919,
			tags: [
				'labore',
				'incididunt',
				'ipsum',
				'aliquip',
				'nisi',
				'aliqua',
				'magna'
			],
			friends: [
				{
					id: 0,
					name: 'Terry Yang'
				},
				{
					id: 1,
					name: 'Dillon Casey'
				},
				{
					id: 2,
					name: 'William Wade'
				}
			]
		}
	}),
	watch: {
		data: {
			handler() {
				this.updated = true

				setInterval(() => {
					this.updated = false
				}, 500)
			},
			deep: true
		}
	}
}
</script>

<style scoped>
main {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	font-size: 16px;
	margin: 15px 10px;
}
section {
	display: grid;
	grid-template-columns: 60% auto;
	grid-column-gap: 15px;
}
fieldset {
	border: 1px solid #ccc;
}
h1 {
	margin: 15px;
	color: gray;
}
legend {
	font-size: 90%;
	color: gray;
}
p {
	display: flex;
	justify-content: space-evenly;
	margin: 0;
}
.grapher {
	margin-top: 15px;
}
textarea {
	border: 0;
	padding: 7px;
	width: calc(100% - 16px);
	height: calc(100% - 24px);
	font-family: inherit;
	font-size: 90%;
	resize: none;
	background-color: inherit;
	color: inherit;
}
.updated {
	box-shadow: 0 0 10px orange;
}
</style>
