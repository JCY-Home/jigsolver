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
	top: 302px;
	left: 145px;
}
p[data-order="2"] {
	top: 302px;
	left: 445px;
}
p[data-order="3"] {
	top: 302px;
	left: 760px;
}
p[data-order="4"] {
	top: 642px;
	left: 145px;
}
p[data-order="5"] {
	top: 642px;
	left: 445px;
}
p[data-order="6"] {
	top: 642px;
	left: 760px;
}
p[data-order="7"] {
	top: 985px;
	left: 145px;
}
p[data-order="8"] {
	top: 985px;
	left: 445px;
}
p[data-order="9"] {
	top: 985px;
	left: 760px;
}
</style>