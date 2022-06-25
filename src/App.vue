<template>
	<div id="app">
        <div class="wrapper clearfix">

            <players 
                v-bind:isWinner="isWinner"
                v-bind:activePlayer="activePlayer"
                v-bind:currentScore="currentScore"
                v-bind:scoresPlayer="scoresPlayer"
            />

            <controls
                v-bind:Rollshake="Rollshake"
                v-bind:isPlaying="isPlaying"
                v-bind:finalScore="finalScore"
                v-on:handleChangeFinalScore="handleChangeFinalScore"
                v-on:handleHoldScore="handleHoldScore"
                v-on:handleNewGame="handleNewGame"
                v-on:handleRollDice="handleRollDice"
            />
            
            <dices 
                v-bind:dices="dices"
            />
            
            <popup-rule 
                v-on:handleConfirm="handleConfirm"
                v-bind:isOpenPopup="isOpenPopup"
            />
             <button class="btn-shake" v-on:click="shakeRolls">Shake to Roll</button>
                <span>absolute:{{absolute}}</span>
                <br>
                <span>alpha:{{alpha}}</span>
                <br>
                <span>beta:{{beta}}</span>
                <br>
                <span>gamma:{{gamma}}</span>
        </div>
	</div>
</template>
 
<script>

import Players from './components/Players';
import Controls from './components/Controls';
import Dices from './components/Dices';
import PopupRule from './components/PopupRule';
// import shake from 'shake.js';

export default {
	name: 'app',
	data () {
		return {
            isPlaying: false,
            isOpenPopup: false,
            activePlayer: 0,
            scoresPlayer: [0, 0],
            dices: [2, 6],
            currentScore: 0,
            finalScore: 10,
            absolute:0,
            alpha:0,
            beta:0,
            gamma:0
		}
	},
	components: {
        Players,
        Controls,
        Dices,
        PopupRule
    },
    computed: {
        isWinner() {
            let { scoresPlayer, finalScore } = this;

            if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
                // Dung cuoc choi
                this.isPlaying = false;
                return true;
            }
            return false;
        }
    },
    mounted() {

// this.shakeRolls()

    },
    methods: {  
           shakeRolls(){
              const handleOrientation = (event) => {
                this.absolute = event.absolute;
                this.alpha = event.alpha;
                this.beta = event.beta;
                this.gamma = event.gamma;
                // alert(alpha +','+ beta +','+gamma)
                // alert('Shake right or left to roll')
                if(this.gamma > 30 && this.gamma < 32){
                    
                        this.handleRollDice()

                
                }else if(this.gamma >-32 && this.gamma < -30){
                    this.handleRollDice()

                }
              
  //...
};

window.addEventListener("deviceorientation", handleOrientation, true);
        },
     
        handleChangeFinalScore(e) {
            var number = parseInt(e.target.value);
            if(isNaN(number)) {
                this.finalScore = '';
            } else {
                this.finalScore = number;
            }
            // console.log();
        },
        handleHoldScore() {
            if(this.isPlaying) {
             
                let { scoresPlayer, activePlayer, currentScore } = this;
                let scoreOld = scoresPlayer[activePlayer];
               
                
                this.$set(this.scoresPlayer, activePlayer, scoreOld + currentScore);

                if(!this.isWinner) {
                    this.nextPlayer();
                }
            
            } else {
                alert('Vui lòng nhất vào nút NewGame');
            }
        },
        nextPlayer() {
            // activePlayer = 0 -> 1  ===== 1 -> 0
            this.activePlayer = this.activePlayer === 0 ? 1 : 0;
            this.currentScore = 0;
        },
        handleRollDice() {
            console.log('handleRollDice App.vue');
            // window.navigator.vibrate(300);
            if(this.isPlaying) {
               
                var dice1 = Math.floor(Math.random() * 6) + 1;
                var dice2 = Math.floor(Math.random() * 6) + 1;

                this.dices = [dice1, dice2];
                console.log(dice1, dice2);

                if(dice1 === 1 || dice2 === 1) {
                    // window.navigator.vibrate(400);
                    let activePlayer = this.activePlayer;
                    setTimeout(function() {
                        alert(`Nguoi choi Player ${activePlayer + 1} đã quay trúng số 1. Rất tiếc`);
                    }, 10)
                    
                    this.nextPlayer();
                } else {
                    this.currentScore = this.currentScore + dice1 + dice2;
                }
            } else {
                alert('Vui lòng nhất vào nút NewGame');
            }
        },
        handleConfirm() {
            this.isPlaying = true;
            this.isOpenPopup = false;
            this.activePlayer = 0;
            this.dices = [1, 1];
            this.scoresPlayer = [0, 0];
            this.currentScore = 0;
        },
        handleNewGame() {
            console.log('handleNewGame App.vue');
            this.isOpenPopup = true;
        }
       
}
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}
.btn-shake{
    padding: 15px 20px;
    border: none;
    background: #42b983;
    color: #333;
    font-size: 20px;
}
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}
</style>
