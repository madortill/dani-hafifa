<template>
  <div id="the-ship">
    <img class="ship" src="../../src/assets/pictures/ship.svg" />
    <img id="player" class="player" :src="playerImageUrl" alt="player" />


<!-- <div
        :style="{ '--light-color': lightColor, '--dark-color': darkColor }"
        class="imposter"
      >
        <div class="spacesuit">
          <div class="chest-and-head"></div>
          <div class="legs"></div>
          <div class="arm"></div>
          <div class="helmet-glass"></div>
        </div>
      </div> -->



    <!-- Rendering the board mat -->
    <div id="board" class="board">
      <div v-for="i in board" :key="i.index" class="row">
        <div v-for="j in i" :key="j.index" class="cell"></div>
      </div>
    </div>

    <!-- the items -->
    <img
      @click="showTheWindow"
      v-if="shownItem === 1"
      class="first-item"
      src="../../src/assets/pictures/glowItems/first.svg"
      alt="glow-table"
    />
    <img
      @click="showTheWindow"
      v-else-if="shownItem === 2"
      class="second-item"
      src="../../src/assets/pictures/glowItems/second.svg"
      alt="glow-table"
    />
    <img
      @click="showTheWindow"
      v-else-if="shownItem === 3"
      class="third-item"
      src="../../src/assets/pictures/glowItems/third.svg"
      alt="glow-machine"
    />

    <!-- joy-stick -->
    <div class="joystick">
      <img
        class="back"
        src="../../src/assets/pictures/moveBtn/background.svg"
        alt="back-joystick"
      />
      <img
        class="move-btn up-btn"
        src="../../src/assets/pictures/moveBtn/button.svg"
        alt="up"
        @click="onkeyDown(2)"
      />
      <img
        class="move-btn down-btn"
        src="../../src/assets/pictures/moveBtn/button.svg"
        alt="down"
         @click="onkeyDown(0)"
      />
      <img
        class="move-btn left-btn"
        src="../../src/assets/pictures/moveBtn/button.svg"
        alt="down"
         @click="onkeyDown(3)"
      />
      <img
        class="move-btn right-btn"
        src="../../src/assets/pictures/moveBtn/button.svg"
        alt="down"
         @click="onkeyDown(1)"
      />
    </div>

    <!-- the children -->
    <what-is-a-meteor
      @close="closeWindow"
      :color="chosenColor"
      v-if="WindowNUM === 1 && toShow"
    ></what-is-a-meteor>

    <meteors
      @close="closeWindow"
      :color="chosenColor"
      v-if="WindowNUM === 2 && toShow"
    ></meteors>

    <explosion-part
      @toGame="toGamePage"
      :theColor="chosenColor"
      v-if="WindowNUM === 3 && toShow"
    ></explosion-part>
  </div>
</template>

<script>
import ExplosionPart from "./ExplosionPart.vue";
import Meteors from "./Meteors.vue";
import WhatIsAMeteor from "./WhatIsAMeteor.vue";

export default {
  name: "the-ship",
  props: ["chosenColor", "lightColor", "darkColor"],
  components: {
    WhatIsAMeteor,
    Meteors,
    ExplosionPart,
  },
  data() {
    return {
      WindowNUM: 0,
      toShow: false,
      shownItem: 1,
      DIR_MAT: [
        [1, 0],
        [0, 1],
        [-1, 0],
        [0, -1],
      ],
      colNum: 10,
      rowNum: 12,
      WALL_SIZE: 1,
      SAFTY_WALL: -1,
      EMPTY_VAL: 0,
      intervalDir: 2,

      newPlayerRow: 0,
      newPlayerCol: 0,
      PLAYER_VAL: 2,
      currPlayerRow: 3,
      currPlayerCol: 5,
      positionXCounter: 50,
      positionYCounter: 30,

      DISTANCE_X: 10,
      DISTANCE_Y: 5,
      board: [
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
        [-1, -1, 0, -1, -1, 0, 0, -1, -1, -1],
        [-1, 0, 0, 0, 0, 2, 0, -1, -1, -1],
        [-1, 0, -1, -1, -1, 0, -1, -1, -1, -1],
        [-1, 0, 0, -1, -1, 0, 0, 0, 0, -1],
        [-1, 0, 0, -1, -1, -1, -1, -1, 0, -1],
        [-1, 0, -1, -1, -1, 0, 0, 0, 0, -1],
        [-1, 0, 0, 0, 0, 0, 0, 0, 0, -1],
        [-1, -1, 0, -1, -1, 0, 0, 0, 0, -1],
        [-1, 0, 0, -1, -1, -1, -1, -1, -1, -1],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
      ],
    };
  },
  mounted() {
    this.initMatrix();
  },
  methods: {
    initMatrix() {
      this.board = [
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
        [-1, -1, 0, -1, -1, 0, 0, -1, -1, -1],
        [-1, 0, 0, 0, 0, 2, 0, -1, -1, -1],
        [-1, 0, -1, -1, -1, 0, -1, -1, -1, -1],
        [-1, 0, 0, -1, -1, 0, 0, 0, 0, -1],
        [-1, 0, 0, -1, -1, -1, -1, -1, 0, -1],
        [-1, 0, -1, -1, -1, 0, 0, 0, 0, -1],
        [-1, 0, 0, 0, 0, 0, 0, 0, 0, -1],
        [-1, -1, 0, -1, -1, 0, 0, 0, 0, -1],
        [-1, 0, 0, -1, -1, -1, -1, -1, -1, -1],
        [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
      ];
    },

    onkeyDown(dir) {
      if (dir >= 0 && dir < 4) {
        this.movePlayer(dir);
      }
    },

    updateBoard() {
      this.board = [...this.board];
    },

    movePlayer(direction) {
      let nextTurn;
      //Calculate the new position of the player based on the direction.
      this.newPlayerRow = this.currPlayerRow + this.DIR_MAT[direction][0];
      this.newPlayerCol = this.currPlayerCol + this.DIR_MAT[direction][1];
      nextTurn = this.board[this.newPlayerRow][this.newPlayerCol];

      //Check what is on the next cell.
      if (nextTurn !== this.SAFTY_WALL) {
        this.board[this.currPlayerRow][this.currPlayerCol] = this.EMPTY_VAL;

        this.currPlayerRow = this.newPlayerRow;
        this.currPlayerCol = this.newPlayerCol;
        this.board[this.currPlayerRow][this.currPlayerCol] = this.PLAYER_VAL;

        this.updateBoard();

        //Update the front matrix
        this.positionXCounter += this.DIR_MAT[direction][1] * this.DISTANCE_X;
        this.positionYCounter += this.DIR_MAT[direction][0] * this.DISTANCE_Y;

        document.getElementById(
          "player"
        ).style.left = `${this.positionXCounter}%`;
        document.getElementById(
          "player"
        ).style.top = `${this.positionYCounter}%`;
      }
    },

    showTheWindow() {
      if (!this.isItemDisabled()) {
        this.shownItem++;
        this.WindowNUM++;
        this.toShow = true;
      }
    },

    closeWindow() {
      this.toShow = false;
    },

    isItemDisabled() {
      let isNearFirst =
        this.shownItem === 1 &&
        (this.currPlayerCol === 1 || this.currPlayerCol === 2) &&
        (this.currPlayerRow === 2 || this.currPlayerRow === 3);
      let isNearSecond =
        this.shownItem === 2 &&
        this.currPlayerCol > 4 &&
        this.currPlayerCol < 9 &&
        this.currPlayerRow === 9;
      let isNearThird =
        this.shownItem === 3 &&
        (this.currPlayerCol === 5 || this.currPlayerCol === 6) &&
        this.currPlayerRow === 2;
      if (isNearFirst || isNearSecond || isNearThird) {
        return false;
      } else {
        return true;
      }
    },

    toGamePage() {
      this.$emit("toGame");
    },
  },

  computed: {
    playerImageUrl() {
      return `src/assets/pictures/players/${this.chosenColor}.svg`;
    },
  },
};
</script>

<style scoped>
body {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0%;
}

/*  */


/*  */



.joystick {
  position: absolute;
  bottom: 0%;
  right: 2%;
  width: 35%;
  height: 16%;
}

.back {
  width: 100%;
}

.move-btn {
  position: absolute;
  width: 35%;
}

.up-btn {
top: 2%;
  left: 50%;
  transform: translateX(-50%);
}

.down-btn {
  left: 50%;
  transform: translateX(-50%) rotate(180deg);
  top: 68%;
}

.left-btn {
  left: 0%;
  transform: translateY(-50%) rotate(270deg);
  top: 50%;
}

.right-btn {
  
  transform: translateY(-50%) rotate(90deg);
  top: 50%;
  right: 0%;
}

.first-item {
  position: absolute;
  top: 25%;
  width: 17%;
  height: 9%;
  left: 4%;
}

.second-item {
  position: absolute;
  bottom: 28%;
  width: 18%;
  height: 9%;
  right: 23%;
}

.third-item {
  position: absolute;
  top: 17%;
  width: 18%;
  height: 13%;
  right: 28%;
}

.ship {
  width: 95%;
  height: 87%;
  position: absolute;
  top: 1%;
  left: 50%;
  transform: translateX(-50%);
}

.board {
  grid-gap: 2px;
  position: absolute;
  top: 15%;
  width: 100%;
  height: 50%;
}

.row {
  display: flex;
  height: 10%;
}

.cell {
  width: 10%;
  height: 100%;
}

.player {
  position: absolute;
  top: 30%;
  left: 50%;
  width: 10vw;
  height: 5vh;
}
</style>