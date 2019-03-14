<template>
	<span>
		<button
			v-for="(type, i) in list.filter(r => r)"
			:key="i"
			@click.prevent="$parent.$emit(type)"
		>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="12"
				height="12"
				viewBox="0 0 24 24"
			>
				<path v-if="type === 'plus'" d="M24 9h-9v-9h-6v9h-9v6h9v9h6v-9h9z" />
				<path v-if="type === 'minus'" d="M0 10h24v4h-24z" />
				<path
					v-if="type === 'close'"
					d="M24 20.188l-8.315-8.209 8.2-8.282-3.697-3.697-8.212 8.318-8.31-8.203-3.666 3.666 8.321 8.24-8.206 8.313 3.666 3.666 8.237-8.318 8.285 8.203z"
				/>
			</svg>
		</button>
	</span>
</template>

<script>
export default { props: { list: Array } }
</script>

<style scoped>
span {
	display: flex;
}
button {
	cursor: pointer;
	text-align: center;
	font-family: inherit;
	border: 0;
	padding: 0;
	width: 20px;
	height: 20px;
	background-color: transparent;
	opacity: 0.4;
}
button:hover {
	opacity: 0.8;
}
</style>
