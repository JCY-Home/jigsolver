<template>
	<div class="puzzle">
		<!-- <draggable v-model="myArray" :options="{draggable:'.item'}">
			<div v-for="element in myArray" :key="element.id" class="item">
				{{element.name}}
			</div>
		</draggable> -->
		<div class="board">
			<div v-for="(piece, $index) in layout" :key="piece.id" class="puzzlePiece" v-on:dragstart = "dragStart($index)"
			v-on:dragenter.prevent = "dragEnter"
			v-on:dragover.prevent = "dragOver"
			v-on:drop.prevent = "dropLeave($index)">
				<img :src="piece"/>
			</div>
			<div class="clearfix"></div>
		</div>
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
		// handleDragStart: function(e) {
		// 	var dragSrcEl = this;

		// 	e.dataTransfer.effectAllowed = 'move';
		// 	e.dataTransfer.setData('text/html', this.innerHTML);
		// },
		// handleDragOver: function(e) {
		// 	if (e.preventDefault) {
		// 		e.preventDefault();
		// 	}
		// 	e.dataTransfer.dropEffect = 'move';

		// 	return false;
		// },
		// handleDrop: function(e) {
		// 	if (e.stopPropagation) {
		// 		e.stopPropagation();
		// 	}
		// 	if (this.handleDragStart.dragSrcEl != event.target) {
		// 		this.handleDragStart.dragSrcEl.innerHTML = event.target.innerHTML;
		// 		event.target.innerHTML = e.dataTransfer.getData('text/html');
		// 	}

		// 	return false;
		// },

		switchEl: function() {
			var temp = this.layout[this.position.sourceIndex];
			this.layout[this.position.sourceIndex] = this.layout[this.position.destIndex];
			this.layout[this.position.destIndex] = temp;
		},
		dragStart(index) {
			var dragItem = event.target;
			event.dataTransfer.setData("URL", dragItem.src);
			event.dataTransfer.setData("text", index);
			event.dataTransfer.effectAllowed = 'move';
		},
		dragEnter() {
			// console.log('enter event');
		},
		dragOver() {
			// console.log('enter event');
		},
		dropLeave(index) {
			var dragItem = event.target;
			let sIndex = event.dataTransfer.getData('text');
			let sURL = event.dataTransfer.getData('URL');
			let toURL = dragItem.src;
			this.$set(this.layout, sIndex, toURL);
			this.$set(this.layout, index, sURL);
			this.isSuccess();
		},
		isSuccess() {
			let puzzleArr = this.layout;
			let isPass = puzzleArr.every((e, i) => e.substr(-5,1) == i + 1);

			if(isPass) {
				setTimeout(function() {
					alert('no idea what this is');
				})
			}
		}
	},
	beforeMount() {
		this.render()
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