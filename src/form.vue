<template>
	<span class="form">
		<span class="flex top">
			<label>
				Type:
				<select v-model="type">
					<option
						v-for="[type, name] in Object.entries(typesFormated)"
						:key="type"
						:value="type"
						v-text="name"
					/>
				</select>
			</label>
			<buttons :list="['close']" />
		</span>

		<label v-if="!isArray">
			Key: <input :class="error.field ? 'error' : null" v-model="field" />
			<span v-if="error.field" class="error" v-text="error.field" />
		</label>

		<label v-if="!['array', 'object'].find(row => row === type)">
			Value:
			<select
				v-if="type === 'boolean'"
				:class="error.value ? 'error' : null"
				v-model="value"
			>
				<option value="true">True</option>
				<option value="false">False</option>
			</select>
			<input
				v-else
				:type="type === 'number' ? 'number' : 'text'"
				step="any"
				:class="error.value ? 'error' : null"
				v-model="value"
			/>
			<span v-if="error.value" class="error" v-text="error.value" />
		</label>

		<span class="flex bottom">
			<button type="button" @click="$parent.$emit('plus')">Cancel</button>
			<button type="button" class="submit" @click="submit">Save</button>
		</span>
	</span>
</template>

<script>
import buttons from './buttons.vue'

export default {
	components: { buttons },
	data: () => ({
		field: null,
		value: null,
		type: 'string',
		error: {},
		types: {
			string: value => value,
			number: value => parseFloat(value),
			boolean: value => value === 'true',
			array: [],
			object: {}
		}
	}),
	computed: {
		typesFormated() {
			return Object.assign(
				...Object.keys(this.types)
					.map(type => ({
						[type]: type.charAt(0).toUpperCase() + type.slice(1)
					}))
					.concat({})
			)
		},
		isArray() {
			return this.$parent.$parent.isArray || this.$parent.type === 'array'
		}
	},
	created() {
		this.$on('close', () => {
			this.$parent.$emit('plus')
		})
	},
	methods: {
		submit() {
			const { field, value, type, types } = this

			if (!this.isArray)
				this.$set(
					this.error,
					'field',
					field === null ? 'This field is required' : null
				)

			this.$set(
				this.error,
				'value',
				value === null && !['array', 'object'].find(row => row === type)
					? 'This field is required'
					: null
			)

			if (!Object.values(this.error).filter(row => row).length) {
				this.$parent.$emit('add', {
					field,
					value:
						types[type] instanceof Function ? types[type](value) : types[type]
				})
			}
		}
	}
}
</script>
