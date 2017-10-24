<template>
	<div class="puzzle-wrap">
		<span v-model="tries" class="counter">Number of tries: {{ tries }}</span>
		<div class="board">
			<div v-for="(piece, $index) in layout" :key="piece.id" class="puzzle-piece" @dragstart="handleDragStart($index)" @dragenter.prevent="handleDragEnter" @dragover.prevent="handleDragOver" @dragleave.prevent="handleDragLeave" @drop.prevent="handleDrop($index)" @dragend.prevent="handleDragEnd" :id="$index">
				<img :src="piece"/>
			</div>
			<labeler/>
			<div class="clearfix"></div>
			<div class="win-box" @click="reloadPage">
				<span>You win!<br>Play again?</span>
			</div>
			<div class="lose-box" @click="reloadPage">
				<span>You lost...<br>Play again?</span>
			</div>
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
			dragTarget: '',
			dropTarget: '',
			tries: 0,
			isWon: false,
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
			    document.getElementById('5').firstChild.getAttribute('src').indexOf('piece5') !== -1,
			    document.getElementById('6').firstChild.getAttribute('src').indexOf('piece9') !== -1,
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
				console.log("turn taken");
				this.dropTarget = '';
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
			this.dragTarget = event.target;
			// event.dataTransfer.setData("URL", dragItem.src);
			event.dataTransfer.setData("text", index);
			event.dataTransfer.effectAllowed = 'move';
		},
		handleDragEnter: function() {
			var enteredItem = event.target;
			enteredItem.classList.add('drag-over');
		},
		handleDragOver: function() {
			// console.log('over has fired');
		},
		handleDragLeave: function() {
			var leftItem = event.target;
			if(leftItem.classList.contains('drag-over')) {
				leftItem.classList.remove('drag-over');
			}
		},
		handleDrop: function(index) {
			console.log('FIRE DROP!');
			this.dropTarget = event.target;
		    var sourceIndex = event.dataTransfer.getData('text'),
		        sourceURL = event.dataTransfer.getData('URL'),
		        destURL = this.dropTarget.src;
			this.$set(this.layout, sourceIndex, destURL);
			this.$set(this.layout, index, sourceURL);
			if(this.dropTarget.classList.contains('drag-over')) {
				this.dropTarget.classList.remove('drag-over');
			}
		},
		handleDragEnd: function() {
			// console.log(this.dragTarget);
			console.log(this.dropTarget);
			if(this.dragTarget === this.dropTarget || !this.dropTarget) {
				// console.log('NO TURN TAKEN');
				return;
			}
			this.checkWinState();
			if(!this.isWon && this.tries < 6) {
				this.tries += 1;
			} else if(this.isWon && this.tries <= 6) {
				this.tries += 1;
				document.querySelector('.counter').classList.add('winner');
				document.querySelector('.win-box').classList.add('show');
				var draggyPieces = document.getElementsByClassName('puzzle-piece');
			    for(var p = 0; p < draggyPieces.length; p++) {
			    	draggyPieces[p].classList.add('game-over');
			    }
			} else {
				this.tries += 1;
				document.querySelector('.counter').classList.add('loser');
			    document.querySelector('.lose-box').classList.add('show');
			    var draggyPieces = document.getElementsByClassName('puzzle-piece');
			    for(var p = 0; p < draggyPieces.length; p++) {
			    	draggyPieces[p].classList.add('game-over');
			    }
			}
		},
		reloadPage: function() {
			window.location.reload();
		}
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
.puzzle-piece {
	float: left;
	width: 300px;
	height: 300px;
	margin-right: 5px;
	margin-left: 5px;
	margin-bottom: 40px;
}
.puzzle-piece img {
	/*-webkit-perspective: 1000;
	-webkit-backface-visibility: hidden;*/
	transition: all 100ms ease-in-out;
	cursor: pointer;
}
.puzzle-piece img:hover {
	/*transform: translateY(-10px);*/
	opacity: 0.7;
}
.drag-over {
	border: 3px dashed #f25f25;
}
.game-over {
	pointer-events: none;
	user-drag: none; 
	user-select: none;
	-moz-user-select: none;
	-webkit-user-drag: none;
	-webkit-user-select: none;
	-ms-user-select: none;
}
.win-box {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	width: 250px;
	height: 100px;
	text-shadow: 5px 10px 10px #000;
	background-color: #FFF;
	border: 2px solid #2bbd00;
	box-shadow: 2px 3px 12px #000;
	opacity: 0;
	z-index: -1;
	cursor: pointer;
}
.win-box > span {
	position: relative;
	top: 50%;
	transform: translateY(-50%);
	display: block;
	font-size: 30px;
	font-weight: bold;
	color: #2bbd00;
	text-shadow: none;
}
.lose-box {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	width: 250px;
	height: 100px;
	text-shadow: 5px 10px 10px #000;
	background-color: #FFF;
	border: 2px solid #EC0202;
	box-shadow: 2px 3px 12px #000;
	opacity: 0;
	z-index: -1;
	cursor: pointer;
}
.lose-box > span {
	position: relative;
	top: 50%;
	transform: translateY(-50%);
	display: block;
	font-size: 30px;
	font-weight: bold;
	color: #EC0202;
	text-shadow: none;
}
.show {
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