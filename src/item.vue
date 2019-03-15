<template>
	<span>
		<span :class="['item', isIterable || left === undefined ? 'parent' : null]">
			<span>
				<span
					v-if="!$parent.isArray && left"
					v-text="left"
					:contenteditable="$parent.editable"
					@blur="leftModel = $event.target.innerText"
					@keydown.enter.prevent="$event.target.blur()"
					@keydown.esc.prevent="$event.target.blur()"
				/>{{
					''.concat(
						!$parent.isArray && left !== undefined ? ':' : '',
						openBracket
					)
				}}
				<span>
					<span
						v-if="!isIterable && right !== undefined"
						v-text="right === null ? 'null' : right"
						:contenteditable="$parent.editable"
						:class="type"
						@blur="rightModel = $event.target.innerText"
						@keydown.enter.prevent="$event.target.blur()"
						@keydown.esc.prevent="$event.target.blur()"
					/>{{
						!isLast && !isIterable && right !== undefined && $parent.comma
							? ','
							: ''
					}}
				</span>
			</span>

			<buttons
				v-if="$parent.editable"
				:list="[
					isIterable || left === undefined ? 'plus' : null,
					left ? 'minus' : null
				]"
			/>
		</span>
		<Form v-if="add" />
	</span>
</template>

<script>
import Buttons from './buttons.vue'
import Form from './form.vue'

export default {
	components: { Buttons, Form },
	props: {
		value: null,
		left: undefined,
		right: undefined,
		isLast: {
			type: Boolean,
			default: false
		}
	},
	data: () => ({ add: false }),
	computed: {
		type() {
			const { rightModel: value } = this

			if (value === null) return 'null'
			if (/@/.test(value)) return 'email'
			if (/:\/\//.test(value)) return 'url'
			if (Array.isArray(value)) return 'array'

			return typeof value
		},
		leftModel: {
			get() {
				return this.left
			},
			set(field) {
				if (field !== this.left)
					this.$emit('input', {
						field,
						value: this.rightModel,
						isField: this.left
					})
			}
		},
		rightModel: {
			get() {
				return this.right
			},
			set(val) {
				let value = val

				if (value && !Number.isNaN(Number(value))) value = parseFloat(value)

				if (value === 'true') value = true
				if (value === 'false') value = false
				if (value === 'null') value = null

				if (value !== this.right)
					this.$emit('input', { field: this.leftModel, value })
			}
		},
		isIterable() {
			return (
				this.rightModel && ['array', 'object'].find(row => row === this.type)
			)
		},
		openBracket() {
			const {
				left,
				type,
				$parent: { brackets, isArray }
			} = this

			if (!brackets) return ''

			if (type === 'array' || (isArray && left === undefined)) return ' ['
			if (type === 'object' || (!isArray && left === undefined)) return ' {'

			return ''
		}
	},
	created() {
		this.$on('plus', () => {
			this.add = !this.add
		})

		this.$on('minus', () => {
			this.$emit('input', { isField: this.left })
		})

		this.$on('add', ({ field, value }) => {
			if (this.isIterable) {
				this.$emit('input', {
					field: this.left,
					value:
						this.type === 'array'
							? [...this.rightModel].concat(value)
							: Object.assign({ ...this.rightModel }, { [field]: value })
				})
			} else {
				this.$emit('input', { field, value })
			}

			this.$emit('plus')
		})
	}
}
</script>

<style scoped>
*[contenteditable]:empty {
	min-width: 1px;
	display: inline-block;
}
.item {
	display: block;
	padding: 3px 0;
	display: flex;
	justify-content: space-between;
}
.string {
	color: seagreen;
}
.number {
	color: darkorange;
}
.email {
	color: goldenrod;
}
.boolean {
	color: royalblue;
}
.null {
	color: royalblue;
}
.url {
	color: cornflowerblue;
}
</style>
