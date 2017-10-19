<template>
	<div class="puzzle">
		<!-- <draggable v-model="myArray" :options="{draggable:'.item'}">
			<div v-for="element in myArray" :key="element.id" class="item">
				{{element.name}}
			</div>
		</draggable> -->
		<div class="board">
			<draggable v-model="puzzle" :options="{draggable:'.dragger'}">
				<div v-for="piece in puzzle" :key="piece.id" class="dragger">
					<img :src="piece"/>
				</div>
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
			puzzle: [],
		}
	},
	methods: {
		render() {
			var puzzleArr = [],
			    i = 1;
			for(i; i < 16; i++) {
				puzzleArr.push("../static/pieces/piece"+i+".png");
			}
			puzzleArr = puzzleArr.sort(function() {
	            return Math.random() - 0.5;
	        });
				
	        this.puzzle = puzzleArr;
		},
	},
	beforeMount() {
		this.render()
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
/*.blank {
	display: inline-block;
	background-color: red;
	width: 225px;
	height: 225px;
}*/
</style>