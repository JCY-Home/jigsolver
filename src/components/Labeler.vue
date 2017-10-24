<template>
	<div class="blank">
		<p class="label-name" v-for="(person, $index) in designers" :key="designers.id" :data-order="$index + 1">{{ person }}</p>
	</div>
</template>

<script>
export default {
	name: 'Labeler',
	data() {
		return {
			designers: [
				'Ricardo',
				'Shannon',
				'Phoebe',
				'Zach',
				'Chantal',
				'Emma',
				'Carl',
				'Cara',
				'Sabrina'
			],
		}
	},
}
</script>

<style scoped>
.label-name {
	position: absolute;
	color: #fff;
}
p[data-order="1"] {
	top: 203px;
	left: 82px;
}
p[data-order="2"] {
	top: 203px;
	left: 296px;
}
p[data-order="3"] {
	top: 203px;
	left: 508px;
}
p[data-order="4"] {
	top: 440px;
	left: 82px;
}
p[data-order="5"] {
	top: 440px;
	left: 296px;
}
p[data-order="6"] {
	top: 440px;
	left: 508px;
}
p[data-order="7"] {
	top: 680px;
	left: 82px;
}
p[data-order="8"] {
	top: 680px;
	left: 296px;
}
p[data-order="9"] {
	top: 680px;
	left: 508px;
}
</style>