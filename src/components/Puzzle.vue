<template>
	<div class="puzzle">
		<div class="top">
			<h4>{{ instructions }}</h4>
			<ul>
				<li v-for="item in items">{{ item }}</li>
			</ul>
		</div>
		<div class="board">
			<img v-for="n in 15" :src="require('@/assets/pieces/' + 'piece' + n + '.png')"/>
			<span class="blank"></span>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			instructions: 'Rearrange the pieces to complete the puzzle!',
			items: [
				'Click the piece you want to move',
				'It will go to the empty spot'
			]
		}
	},
	name: 'Puzzle'
}
</script>

<style scoped>
.top {
	width: 300px;
	margin: 0 auto 30px auto;
}
.top ul {
	text-align: left;
}
.board {
	width: 980px;
	column-count: 4;
	border: 1px solid #000;
	background-color: #808080;
	margin: 0 auto;
	padding: 20px;

}
.board img {
	margin-bottom: 10px;
	-webkit-transition: all 100ms ease-in-out;
	transition: all 100ms ease-in-out;
}
.board img:hover {
	-webkit-transform: translateY(-10px);
	        transform: translateY(-10px);
}
.blank {
	display: inline-block;
	background-color: red;
	width: 225px;
	height: 225px;
}
</style>