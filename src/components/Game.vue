<template>
  <div id="game">
    <div v-if="part === 1" class="instructions">
      <!-- <audio ref="audioElement" loop>
        <source src="@/assets/audio/among-us-typing.mp3" type="audio/mpeg">
        your browser does not support audio tag
      </audio> -->
      <div class="finale-exe type-writer">
        <p
          v-for="(text, index) in array1"
          :key="text"
          :style="{
            '--delay': `${index * 2}s`,
            '--width': `${text.length}ch`,
          }"
          class="text-writer"
        >
          {{ text }}
        </p>
      </div>

      <img
        class="arrow"
        @click="next"
        src="@/assets/pictures/arrow.svg"
        alt="arrow"
      />
    </div>
    <!-- the grid -->
    <div class="container" v-if="part === 2">
      <div :style="{ '--chosen-color': theColor }" id="coordinates">
        {{ "( " + this.prevX + " , " + this.prevY + " )" }}
      </div>
      <div :style="{ '--chosen-color': theColor }" id="timer">
        00:0{{ seconds }}
      </div>

      <!-- Rendering the board mat -->
      <div id="board" class="board" @click="checkPlace">
        <div v-for="i in 5" :key="i" class="row">
          <div
            v-for="j in 5"
            :key="j - 1 + ' ' + Math.abs(i - 5)"
            :id="j - 1 + ' ' + Math.abs(i - 5)"
            class="cell"
          ></div>
        </div>
      </div>

      <img
        class="explosion"
        v-if="showExplosion"
        src="src/assets/pictures/explosion.gif"
        alt="explosion"
      />

      <div class="life-container">
        <img
          v-for="i in 3"
          class="life"
          src="@/assets/pictures/heart.svg"
          alt="heart"
          :key="i"
          :id="i"
        />
      </div>
      <div class="score-container">
        <img class="trophy" src="src/assets/pictures/trophy.png" alt="trophy" />
        <p class="the-score">{{ score }} / 5</p>
      </div>
    </div>

    <end-game
    :lightColor="lightColor"
    :darkColor="darkColor"
      :isWin="isAWin"
      @hideBackGround="hideTheBG"
      @showBackGround="showTheBG"
      @toNextPage="toTheNextPage"
      @again="restart"
      v-if="part === 3"
    ></end-game>
  </div>
</template>

<script>
import EndGame from "./EndGame.vue";

export default {
  name: "game",
  components: { EndGame },
  props: ["theColor", "lightColor", "darkColor"],
  data() {
    return {
      array1: [
        "עכשיו נבדוק את המיומנות שלך.",
        "במסך הבא יופיע הקורדינאטות ",
        "שתחנת השיגור שלחה לך. ",
        "הן מסמלות את מיקום ",
        "האסטרואיד העויין ",
        "ועלייך ללחוץ במסך על ",
        "המיקום הנכון כדי לפוצץ אותו.",
        "יש לך 3 חיים ובמידה והמיקום ",
        "לא נכון אז ירד לך חיים אחד.",
        "בהצלחה!!",
      ],
      showArrow: false,
      numRowsCols: 4,
      prevX: Math.round(Math.random() * (4 - 0) + 0),
      prevY: Math.round(Math.random() * (4 - 0) + 0),
      part: 1,
      roundNum: 0,
      life: 3,
      showExplosion: false,
      score: 0,
      seconds: 8,
      isAWin: true,
      allowClick: true,
      isClicked: false,
      theTimer: 0,
    };
  },
  methods: {
    toTheNextPage() {
      this.$emit("toFinishPage");
    },
    hideTheBG() {
      this.$emit("toUnshowBG");
    },
    showTheBG() {
      this.$emit("toshowBG");
    },
    next() {
      this.part++;
      if (this.roundNum < 5) {
        this.timer();
      }
    },
    restart() {
      this.theCoordinates();
      this.part = 1;
      this.score = 0;
      this.roundNum = 0;
      this.life = 3;
      this.score = 0;
      this.isAWin = true;
      this.allowClick = true;
      this.isClicked = false;
    },
    checkPlace(event) {
      this.roundNum++;
      if (this.roundNum < 5) {
        if (
          event.target.id === this.prevX + " " + this.prevY &&
          !this.showExplosion
        ) {
          this.showExplosion = true;
          this.allowClick = false;
          this.score++;

          let explosion = setTimeout(() => {
            this.showExplosion = false;
            this.allowClick = true;
            this.timer();
            this.theCoordinates();
            clearTimeout(explosion);
          }, 2000);

          
        } else {
          this.lifeCheck();
        }
      } else {
            this.isAWin = true;
            this.next();
          }
    },
    theCoordinates() {
      //new values
      let newX = Math.round(Math.random() * (this.numRowsCols - 0) + 0);
      let newY = Math.round(Math.random() * (this.numRowsCols - 0) + 0);
      // cheking if the values are the same
      while (this.prevX === newX && this.prevY === newY) {
        newX = Math.round(Math.random() * (this.numRowsCols - 0) + 0);
        newY = Math.round(Math.random() * (this.numRowsCols - 0) + 0);
      }
      this.prevX = newX;
      this.prevY = newY;
      return "( " + newX + " , " + newY + " )";
    },
    lifeCheck(isWin) {
      if (this.life > 0) {
        document.getElementById(this.life).remove();
        this.life--;
        this.timer();
        this.theCoordinates();
        if (this.life === 0) {
          this.isAWin = false;
          this.next();
        }
      }
    },
    timer() {
      if (this.allowClick) {
        this.seconds = 8;
        //but do a tnai id there was a click
        clearInterval(this.theTimer);
        this.theTimer = setInterval(() => {
          
          if (this.seconds > 0) {
            this.seconds--;
            return "00:0" + this.seconds;
          } else if (this.seconds === 0) {
            if (this.allowClick) {
              this.lifeCheck();
            }
            clearInterval(this.theTimer);
          }
        }, 1000);

        return "00:0" + this.seconds;
      }
    },
  },
};
</script>


<style scoped>
body {
  width: 100vw;
  height: 100vh;
}

#game {
  top: 0%;
  overflow: hidden;
}

.text {
  font-family: bn pixeliom;
  color: white;
  position: absolute;
  top: 8%;
  left: 50%;
  transform: translateX(-50%);
  font-size: 3vh;
  width: 95%;
  direction: rtl;
}

.arrow {
  width: 20vw;
  height: 9vh;
  position: absolute;
  bottom: 5%;
  left: 50%;
  transform: translateX(-50%);
  animation: float 2s linear infinite;
}

@keyframes float {
  0% {
    bottom: 5%;
  }

  50% {
    bottom: 8%;
  }

  100% {
    bottom: 5%;
  }
}

@-webkit-keyframes float {
  0% {
    bottom: 5%;
  }

  50% {
    bottom: 8%;
  }

  100% {
    bottom: 5%;
  }
}

.instructions {
  width: 100vw;
  height: 100vh;
}

.finale-exe {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  height: 70%;
  bottom: 20%;
  border-radius: 50px;
  box-shadow: 0 15px 20px -20px rgba(0, 0, 0, 0.4);
  padding: 0 2rem;
  direction: rtl;
  font-size: 6.5vw;
  font-family: bn pixeliom;
}

.type-writer > p {
  animation: typeWrite 2s var(--delay) steps(44) normal both,
    blink 400ms var(--delay) steps(45) 6;
}

@keyframes typeWrite {
  from {
    width: 0%;
  }

  to {
    width: var(--width);
  }
}

.finale-exe > p {
  display: block;
  width: fit-content;
  white-space: nowrap;
  text-align: right;
  overflow: hidden;
  border-left: solid 3px transparent;
  max-width: fit-content;
  margin: 4%;
}

@keyframes blink {
  to {
    border-left-color: black;
  }

  from {
    border-left-color: transparent;
  }
}

.text-writer {
  text-align: right;
}

#coordinates {
  color: white;
  border-color: var(--chosen-color);
  background-color: var(--chosen-color);
  grid-column: 1 / 6;
  position: relative;
  color: black;
}

#timer {
  background-color: #292929;
  grid-column: 6 / 11;
  color: var(--chosen-color);
}

.board {
  grid-row: 3/9;
  grid-column: 2/10;
}

.row {
  display: flex;
  height: 20%;
}

.cell {
  width: 20%;
  height: 100%;
  border: 2px solid white;
}

.container {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0%;
  right: 0%;
  display: grid;
  grid-template-columns: repeat(10, 10%);
  grid-template-rows: repeat(10, 10%);
  font-size: 15vw;
  text-align: center;
}

.life-container {
  grid-column: 1 / 6;
  grid-row: 10;
  position: relative;
  top: 2vh;
}

.life {
  width: 31%;
  margin: 1%;
}

.score-container {
  grid-column: 1 / 6;
  grid-row: 9;
}

.the-score {
  font-size: 15vw;
  position: relative;
  font-weight: bold;
  bottom: 13vh;
  left: 10vw;
}

.trophy {
  width: 40%;
  position: relative;
  top: 2vh;
  right: 15vw;
}

.explosion {
  grid-row: 3/9;
  grid-column: 2/10;
  width: 100%;
  height: 70%;
  position: relative;
  top: 15%;
}
</style>