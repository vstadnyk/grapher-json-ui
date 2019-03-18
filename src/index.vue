<template>
	<ul v-if="data" class="grapher-json-ui">
		<li v-if="isRoot"><item v-model="data" /></li>
		<li
			v-for="([field, value], i) in Object.entries(data)"
			:key="i"
			:class="!isRoot || brackets ? 'tab' : null"
		>
			<item
				v-model="data"
				:left="field"
				:right="value"
				:isLast="i === size - 1"
			/>

			<grapher-json-ui
				v-model="data[field]"
				v-bind="{ editable, brackets, comma }"
				:isLast="i === size - 1"
			/>
		</li>
		<li>
			{{
				brackets
					? (isArray ? ']' : '}').concat(!isLast && comma ? ',' : '')
					: null
			}}
		</li>
	</ul>
</template>

<script>
import Item from './item.vue'

import './index.css'

export default {
	name: 'grapher-json-ui',
	components: { Item },
	props: {
		value: null,
		editable: {
			type: Boolean,
			default: false
		},
		brackets: {
			type: Boolean,
			default: true
		},
		comma: {
			type: Boolean,
			default: true
		},
		isLast: {
			type: Boolean,
			default: true
		}
	},
	computed: {
		isArray() {
			return Array.isArray(this.data)
		},
		isIterable() {
			return this.data && typeof this.data === 'object'
		},
		isRoot() {
			return this.$parent.$options.name !== this.$options.name
		},
		size() {
			return Object.keys(this.data || {}).length
		},
		data: {
			get() {
				if (this.value && typeof this.value === 'object') return this.value

				return null
			},
			set({ field, value, isField = false }) {
				const data = this.isArray ? [...this.value] : { ...this.value }

				if (this.isArray) {
					this.$emit('input', data.concat(value))
				} else {
					if (field && value !== undefined)
						Object.assign(data, { [field]: value })

					const fields = Object.keys(data)

					if (isField) {
						const index = fields.indexOf(isField)

						if (field) {
							fields[index] = field
						} else {
							fields.splice(index, 1)
						}
					}

					this.$emit(
						'input',
						Object.assign(...fields.map(i => ({ [i]: data[i] })))
					)
				}
			}
		}
	}
}
</script>
