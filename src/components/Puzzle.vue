<template>
	<div class="board">
		<div v-for="(piece, $index) in layout" :key="piece.id" class="puzzlePiece" @dragstart="handleDragStart($index)" @dragenter.prevent="handleDragEnter" @dragover.prevent="handleDragOver" @dragleave.prevent="handleDragLeave" @drop.prevent="handleDrop($index)">
			<img :src="piece"/>
		</div>
		<div class="clearfix"></div>
	</div>
</template>

<script>
export default {
	name: 'Puzzle',
	data() {
		return {
			layout: [],
		}
	},
	methods: {
		render: function() {
			var puzzleTemp = [],
			    x = 1;
			for(x; x < 17; x++) {
				puzzleTemp.push("../static/pieces/piece" + x + ".png");
			}
			puzzleTemp = puzzleTemp.sort(function() {
	      return Math.random() - 0.5;
	    });

			this.layout = puzzleTemp;
		},
		handleDragStart: function(index) {
			var dragItem = event.target;
			// event.dataTransfer.setData("URL", dragItem.src);
			event.dataTransfer.setData("text", index);
			event.dataTransfer.effectAllowed = 'move';
		},
		handleDragEnter: function() {
			var enteredItem = event.target;
			enteredItem.classList.add('over');
		},
		handleDragOver: function() {
			// console.log('over has fired');
		},
		handleDragLeave: function() {
			var leftItem = event.target;
			if(leftItem.classList.contains('over')) {
				leftItem.classList.remove('over');
			}
		},
		handleDrop: function(index) {
			var dragItem = event.target,
			    sourceIndex = event.dataTransfer.getData('text'),
			    sourceURL = event.dataTransfer.getData('URL'),
			    destURL = dragItem.src;
			this.$set(this.layout, sourceIndex, destURL);
			this.$set(this.layout, index, sourceURL);
			if(dragItem.classList.contains('over')) {
				dragItem.classList.remove('over');
			}
		},
	},
	beforeMount() {
		this.render()
	},
}
</script>

<style scoped>
.board {
	width: 980px;
	border: 1px solid #000;
	background-color: #808080;
	padding: 10px;
	margin: 0 auto;
}
.puzzlePiece {
	float: left;
	width: 225px;
	height: 225px;
	margin-right: 10px;
	margin-left: 10px;
	margin-bottom: 20px;
}
.puzzlePiece img {
	-webkit-perspective: 1000;
	-webkit-backface-visibility: hidden;
	transition: all 100ms ease-in-out;
	cursor: pointer;
}
.puzzlePiece img:hover {
	transform: translateY(-10px);
}
.over {
	border: 3px dashed #f25f25;
}
.clearfix {
	display: table;
	clear: both;
}
</style>