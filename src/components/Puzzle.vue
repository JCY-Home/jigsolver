<template>
	<div class="puzzle">
		<div class="board">
			<img v-for="n in 15" :src="require('@/assets/pieces/' + 'piece' + n + '.png')" @click="trigger"/>
			<span class="blank"></span>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Puzzle',
	methods: {
		getOffset(el) {
			el = el.getBoundingClientRect();
			return {
				left: el.left + window.scrollX,
				top: el.top + window.scrollY
			}
		},
		trigger(e) {
			var blank = document.querySelector('.blank'),
			    clicked = e.target,
			    blankOffset = this.getOffset(blank).top + this.getOffset(blank).left,
			    clickedOffset = this.getOffset(clicked).top + this.getOffset(clicked).left;
			console.log(blankOffset + ", " + clickedOffset + ", " + (blankOffset - clickedOffset));
			var distance = (blankOffset - clickedOffset) < 260 ? 'adjacent' : 'far';
			console.log(distance);
		}
	}
}
</script>

<style scoped>
.board {
	width: 980px;
	column-count: 4;
	border: 1px solid #000;
	background-color: #808080;
	padding: 20px;
	margin: 0 auto;
}
.board img {
	margin-bottom: 10px;
	-webkit-perspective: 1000;
	-webkit-backface-visibility: hidden;
	transition: all 100ms ease-in-out;
	cursor: pointer;
}
.board img:hover {
	transform: translateY(-10px);
}
.blank {
	display: inline-block;
	background-color: red;
	width: 225px;
	height: 225px;
}
</style>