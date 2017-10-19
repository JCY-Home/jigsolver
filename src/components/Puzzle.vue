<template>
	<div class="puzzle">
		<!-- <draggable v-model="myArray" :options="{draggable:'.item'}">
			<div v-for="element in myArray" :key="element.id" class="item">
				{{element.name}}
			</div>
		</draggable> -->
		<div class="board">
			<draggable :options="{draggable:'.piece'}">
				<img v-for="n in 15" :src="require('@/assets/pieces/' + 'piece' + n + '.png')" class="piece"/>
				<span class="piece blank"></span>
			</draggable>
		</div>
		
	</div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
	name: 'Puzzle',
	data() {
		return {
			isAdjacent: false,
			myArray: [
				{
					"name": 'justin',
					"order": 1,
					"fixed": false
				},
				{
					"name": 'camille',
					"order": 3,
					"fixed": false
				},
				{
					"name": 'nick',
					"order": 2,
					"fixed": false
				}
			],
		}
	},
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
			    clickedOffset = this.getOffset(clicked).top + this.getOffset(clicked).left,
			    distance = (blankOffset - clickedOffset) < 260 ? this.isAdjacent = true : this.isAdjacent = false;
			if(this.isAdjacent) {
				console.log('adjacent');
			} else {
				console.log('not adjacent');
			}
		}
	},
	components: {
		draggable,
	},
}
</script>

<style scoped>
.item {
	border: 1px solid #efefef;
	width: 20%;
	margin: 10px auto;
}
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