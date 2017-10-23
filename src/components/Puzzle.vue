<template>
	<div class="puzzle-wrap">
		<span v-model="tries" class="counter">Number of tries: {{ tries }}</span>
		<div class="board">
			<div v-for="(piece, $index) in layout" :key="piece.id" class="puzzlePiece" @dragstart="handleDragStart($index)" @dragenter.prevent="handleDragEnter" @dragover.prevent="handleDragOver" @dragleave.prevent="handleDragLeave" @drop.prevent="handleDrop($index)" @dragend.prevent="handleDragEnd" :id="$index">
				<img :src="piece"/>
			</div>
			<labeler/>
			<span class="win-message">YOU WIN!</span>
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
			tries: 0,
			wrongItems: {}
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
			var doesWin = [
			    document.getElementById('0').firstChild.getAttribute('src').indexOf('piece2') !== -1,
			    document.getElementById('1').firstChild.getAttribute('src').indexOf('piece7') !== -1,
			    document.getElementById('2').firstChild.getAttribute('src').indexOf('piece3') !== -1,
			    document.getElementById('3').firstChild.getAttribute('src').indexOf('piece4') !== -1,
			    document.getElementById('4').firstChild.getAttribute('src').indexOf('piece8') !== -1,
			    document.getElementById('5').firstChild.getAttribute('src').indexOf('piece9') !== -1,
			    document.getElementById('6').firstChild.getAttribute('src').indexOf('piece5') !== -1,
			    document.getElementById('7').firstChild.getAttribute('src').indexOf('piece6') !== -1,
			    document.getElementById('8').firstChild.getAttribute('src').indexOf('piece1') !== -1
			    ],
			    winChecker = doesWin.every(allAreTrue),
			    wrongOnes = doesWin.every(whichAreFalse);

			function allAreTrue(element, index, array) {
				return element === true;
			}
			function whichAreFalse(element, index, array) {
				return element === false;
			}

			if(!winChecker) {
				console.log("game lost");
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
				this.tries += 1;
				document.querySelector('.counter').classList.add('winner');
				document.querySelector('.puzzle-wrap').classList.add('no-click');
				document.querySelector('.win-message').classList.add('win-show');
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
	position: relative;
	display: inline-block;
	font-size: 28px;
	margin-bottom: 20px;
	z-index: 500;
}
.no-click {
	pointer-events: none;
}
.winner {
	color: #3FFF07 !important;
	-webkit-animation: rotate-scale-up 0.65s linear 3 both;
	        animation: rotate-scale-up 0.65s linear 3 both;
}
.loser {
	color: #ff0707 !important;
	-webkit-animation: wobble-hor-bottom 0.8s 3 both;
	        animation: wobble-hor-bottom 0.8s 3 both;
}
.winner, .loser {
	font-size: 32px;
	font-weight: bold;
	text-shadow: 1px 2px 3px #000;
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
.win-message {
	position: absolute;
	top: 360px;
	left: 105px;
	font-size: 150px;
	font-weight: bold;
	text-shadow: 5px 10px 10px #000;
	color: #3FFF07;
	opacity: 0;
	z-index: -1;
	transform: rotate(-25deg);
}
.win-show {
	opacity: 1 !important;
	z-index: 500;
	transition: all 2s ease-in-out;
}
.clearfix {
	display: table;
	clear: both;
}
@-webkit-keyframes rotate-scale-up {
  0% {
    -webkit-transform: scale(1) rotateZ(0);
            transform: scale(1) rotateZ(0);
  }
  50% {
    -webkit-transform: scale(2) rotateZ(180deg);
            transform: scale(2) rotateZ(180deg);
  }
  100% {
    -webkit-transform: scale(1) rotateZ(360deg);
            transform: scale(1) rotateZ(360deg);
  }
}
@keyframes rotate-scale-up {
  0% {
    -webkit-transform: scale(1) rotateZ(0);
            transform: scale(1) rotateZ(0);
  }
  50% {
    -webkit-transform: scale(2) rotateZ(180deg);
            transform: scale(2) rotateZ(180deg);
  }
  100% {
    -webkit-transform: scale(1) rotateZ(360deg);
            transform: scale(1) rotateZ(360deg);
  }
}
@-webkit-keyframes wobble-hor-bottom {
  0%,
  100% {
    -webkit-transform: translateX(0%);
            transform: translateX(0%);
    -webkit-transform-origin: 50% 50%;
            transform-origin: 50% 50%;
  }
  15% {
    -webkit-transform: translateX(-30px) rotate(-6deg);
            transform: translateX(-30px) rotate(-6deg);
  }
  30% {
    -webkit-transform: translateX(15px) rotate(6deg);
            transform: translateX(15px) rotate(6deg);
  }
  45% {
    -webkit-transform: translateX(-15px) rotate(-3.6deg);
            transform: translateX(-15px) rotate(-3.6deg);
  }
  60% {
    -webkit-transform: translateX(9px) rotate(2.4deg);
            transform: translateX(9px) rotate(2.4deg);
  }
  75% {
    -webkit-transform: translateX(-6px) rotate(-1.2deg);
            transform: translateX(-6px) rotate(-1.2deg);
  }
}
@keyframes wobble-hor-bottom {
  0%,
  100% {
    -webkit-transform: translateX(0%);
            transform: translateX(0%);
    -webkit-transform-origin: 50% 50%;
            transform-origin: 50% 50%;
  }
  15% {
    -webkit-transform: translateX(-30px) rotate(-6deg);
            transform: translateX(-30px) rotate(-6deg);
  }
  30% {
    -webkit-transform: translateX(15px) rotate(6deg);
            transform: translateX(15px) rotate(6deg);
  }
  45% {
    -webkit-transform: translateX(-15px) rotate(-3.6deg);
            transform: translateX(-15px) rotate(-3.6deg);
  }
  60% {
    -webkit-transform: translateX(9px) rotate(2.4deg);
            transform: translateX(9px) rotate(2.4deg);
  }
  75% {
    -webkit-transform: translateX(-6px) rotate(-1.2deg);
            transform: translateX(-6px) rotate(-1.2deg);
  }
}

</style>