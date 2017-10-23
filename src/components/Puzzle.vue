<template>
	<div class="puzzle-wrap">
		<span v-model="tries" class="counter">Number of tries: {{ tries }}</span>
		<div class="board">
			<div v-for="(piece, $index) in layout" :key="piece.id" class="puzzlePiece" @dragstart="handleDragStart($index)" @dragenter.prevent="handleDragEnter" @dragover.prevent="handleDragOver" @dragleave.prevent="handleDragLeave" @drop.prevent="handleDrop($index)" @dragend.prevent="handleDragEnd" :id="$index">
				<img :src="piece"/>
			</div>
			<labeler/>
			<div class="clearfix"></div>
		</div>
	</div>
</template>

<script>
import Labeler from '../components/Labeler'

export default {
	name: 'Puzzle',
	components: {
		Labeler
	},
	data() {
		return {
			layout: [],
			isWon: false,
			tries: 0
		}
	},
	methods: {
		build: function() {
			var puzzleTemp = [];

			for(var x = 1; x < 10; x++) {
				puzzleTemp.push("../static/tr-pieces/piece" + x + ".png");
			}

			puzzleTemp = puzzleTemp.sort(function() {
	      return Math.random() - 0.5;
	    });

			this.layout = puzzleTemp;
		},
		checkWinState: function() {
			if(this.isWon) { return; }
			var doesWin = [
			    document.getElementById('0').firstChild.getAttribute('src').indexOf('piece6') !== -1,
			    document.getElementById('1').firstChild.getAttribute('src').indexOf('piece1') !== -1,
			    document.getElementById('2').firstChild.getAttribute('src').indexOf('piece4') !== -1,
			    document.getElementById('3').firstChild.getAttribute('src').indexOf('piece8') !== -1,
			    document.getElementById('4').firstChild.getAttribute('src').indexOf('piece7') !== -1,
			    document.getElementById('5').firstChild.getAttribute('src').indexOf('piece3') !== -1,
			    document.getElementById('6').firstChild.getAttribute('src').indexOf('piece9') !== -1,
			    document.getElementById('7').firstChild.getAttribute('src').indexOf('piece2') !== -1,
			    document.getElementById('8').firstChild.getAttribute('src').indexOf('piece5') !== -1
			];
			function allAreTrue(element, index, array) {
				return element === true;
			}
			var checker = doesWin.every(allAreTrue);
			if(!checker) {
				console.log("doesn't win");
			} else {
				console.log("game won");
				this.isWon = true;
			}
			// var flag = false;
			// for(var i = 0; i < doesWin.length; i++) {
			// 	doesWin[i] === false ? (flag = true) : (flag = false);
			// }
			// if(!flag) {
			// 	this.isWon = true;
			// 	console.log('game won')
			// } else {
			// 	console.log('doesn\'t win');
			// }
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
		handleDragEnd: function() {
			this.checkWinState();
			if(!this.isWon && this.tries < 6) {
				this.tries += 1;
			} else if(this.isWon && this.tries <= 6) {
				document.querySelector('.counter').classList.add('winner');
				document.querySelector('.puzzle-wrap').classList.add('no-click');
			} else {
				this.tries += 1;
				document.querySelector('.counter').classList.add('loser');
				document.querySelector('.puzzle-wrap').classList.add('no-click');
			}
		},
	},
	beforeMount() {
		this.build()
	},
}
</script>

<style scoped>
.counter {
	display: inline-block;
	font-size: 28px;
	margin-bottom: 20px;

}
.no-click {
	pointer-events: none;
}
.winner {
	color: #00d313 !important;
}
.loser {
	color: #ff0707 !important;
}
.winner, .loser {
	font-size: 32px;
	font-weight: bold;
}
.board {
	position: relative;
	width: 930px;
	border: 1px solid #000;
	background-color: #808080;
	padding: 10px;
	margin: 0 auto;
}
.puzzlePiece {
	float: left;
	width: 300px;
	height: 300px;
	margin-right: 5px;
	margin-left: 5px;
	margin-bottom: 40px;
}
.puzzlePiece img {
	/*-webkit-perspective: 1000;
	-webkit-backface-visibility: hidden;*/
	transition: all 100ms ease-in-out;
	cursor: pointer;
}
.puzzlePiece img:hover {
	/*transform: translateY(-10px);*/
	opacity: 0.7;
}
.over {
	border: 3px dashed #f25f25;
}
#label {

}
.clearfix {
	display: table;
	clear: both;
}
</style>