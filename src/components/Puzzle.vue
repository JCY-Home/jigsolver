<template>
	<div class="puzzle">
		<!-- <draggable v-model="myArray" :options="{draggable:'.item'}">
			<div v-for="element in myArray" :key="element.id" class="item">
				{{element.name}}
			</div>
		</draggable> -->
		<div class="board">
			<draggable v-model="layout" :options="{draggable:'.puzzlePiece'}">
				<div v-for="piece in layout" :key="piece.id" class="puzzlePiece">
					<img :src="piece"/>
				</div>
			</draggable>
			<div class="clearfix"></div>
		</div>
	</div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
	name: 'Puzzle',
	data() {
		return {
			layout: [],
		}
	},
	methods: {
		render() {
			var puzzleArr = [],
			    i = 1;
			for(i; i < 17; i++) {
				puzzleArr.push("../static/pieces/piece"+i+".png");
			}
			puzzleArr = puzzleArr.sort(function() {
	            return Math.random() - 0.5;
	        });
			this.layout = puzzleArr;
		},
		// dragStart(index) {
		// 	var dragItem = event.target;
		// 	event.dataTransfer.setData("URL", dragItem.src);
		// 	event.dataTransfer.setData("text", index);
		// 	event.dataTransfer.effectAllowed = 'move';
		// },
		// dragEnter() {
		// 	console.log('enter event');
		// },
		// dragOver() {
		// 	console.log('enter event');
		// },
		// dropLeave(index) {
		// 	var dragItem = event.target;
		// 	let sIndex = event.dataTransfer.getData('text');
		// 	let sURL = event.dataTransfer.getData('URL');
		// 	let toURL = dragItem.src;
		// 	this.layout.$set(sIndex, toURL);
		// 	this.layout.$set(index, sURL);
		// 	this.isSuccess();
		// },
		// isSuccess() {
		// 	let puzzleArr = this.layout;
		// 	let isPass = puzzleArr.every((e, i) => e.substr(-5,1) == i + 1);

		// 	if(isPass) {
		// 		setTimeout(function() {
		// 			alert('no idea what this is');
		// 		})
		// 	}
		// }
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
.puzzle {
	width: 980px;
	border: 1px solid #000;
	background-color: #808080;
	padding: 20px;
	margin: 0 auto;
}
.puzzlePiece {
	float: left;
	margin-right: 20px;
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
.clearfix {
	display: table;
	clear: both;
}
</style>