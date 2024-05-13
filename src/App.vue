<template>
  <div class="tic-tac-toe">
    <div class="player">The player is: {{ turn }}</div>
    <div class="win" :style="msg != '' ? 'display: block;' : ''">
      <div class="box-msg">
        <p class="msg" :id="colorMsg">{{ msg }}</p>
        <button @click="reload()">Play again</button>
      </div>
    </div>
    <h1 class="title"><span>Tic Tac Toe</span> Game</h1>
    <div class="board" ref="cells">
      <button
        class="cell"
        v-for="i in 9"
        :key="i"
        @click="addCellsToBoard($event)"
      ></button>
    </div>
    <footer>
      <p>Created by <a href="https://github.com/NFM0hammed">NFM0hammed</a></p>
    </footer>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      turn: "X",
      switchTurn: false,
      oddsWins: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ],
      msg: "",
      colorMsg: "",
      count: 0,
    };
  },
  methods: {
    addCellsToBoard(e) {
      // To switch between players
      this.turn = this.switchTurn ? "O" : "X";
      // Add cell on the board after click
      e.target.textContent = this.turn;
      // Add color to x | y
      e.target.classList.add(this.turn.toLowerCase());
      // !false = true => !true = false ...etc
      this.switchTurn = !this.switchTurn;
      // Disabled button after click
      e.target.disabled = true;
      // Each time check win after add cell on the board
      this.checkWin();
    },
    checkWin() {
      let boardCells = Array.from(this.$refs.cells.children);
      let oddsWinsLength = this.oddsWins.length;
      for (let i = 0; i < oddsWinsLength; ++i) {
        if (boardCells[this.oddsWins[i][0]].textContent != "") {
          if (
            boardCells[this.oddsWins[i][0]].textContent ==
              boardCells[this.oddsWins[i][1]].textContent &&
            boardCells[this.oddsWins[i][1]].textContent ==
              boardCells[this.oddsWins[i][2]].textContent
          ) {
            this.msg = `The player ${this.turn} is win`;
            this.colorMsg = "win";
            return false;
          }
        }
      }
      // Check if all cells on the board
      boardCells.forEach((cell) => {
        if (cell.textContent != "") {
          this.count++;
        }
      });
      // Means no win
      if (this.count === boardCells.length) {
        this.msg = `No win !`;
        this.colorMsg = "no-win";
      }
      this.count = 0;
    },
    reload() {
      window.location.reload();
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

/*
  Variables
*/
$mobileSize: "(max-width: 767px)";

body {
  background-color: #f9f9f9;
}
#app {
  font-family: "Poppins", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.tic-tac-toe {
  background-color: #fff;
  width: 400px;
  margin: auto;
  padding: 50px;
  border: {
    top: 3px solid #5a9fac;
    radius: 5px;
  }
  @media #{$mobileSize} {
    & {
      width: 250px;
    }
  }
  .player {
    color: #777;
    font: {
      size: 20px;
      weight: 500;
    }
  }
  .win {
    position: fixed;
    display: none;
    background-color: #00000036;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    .box-msg {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      .msg {
        color: #fff;
        font-size: 50px;
        width: 500px;
        padding: 30px 50px;
        @media #{$mobileSize} {
          & {
            width: 250px;
            font-size: 26px;
          }
        }
        &#win {
          background-color: #23d023;
        }
        &#no-win {
          background-color: #0db2db;
        }
      }
      button {
        background-color: #5a9fac;
        color: #fff;
        font-size: 32px;
        padding: 15px 25px;
        border: none;
        cursor: pointer;
        transition: 0.3s ease-in-out;
        @media #{$mobileSize} {
          & {
            font-size: 23px;
          }
        }
        &:hover {
          background-color: #74a8b3;
        }
      }
    }
  }
  .title {
    color: #777;
    margin: 50px auto;
    @media #{$mobileSize} {
      & {
        font-size: 23px;
      }
    }
    span {
      position: relative;
      color: #5a9fac;
      &::after {
        position: absolute;
        content: "";
        background-color: #5a9fac;
        width: 100%;
        height: 4px;
        bottom: -8px;
        left: 0;
      }
    }
  }
  .board {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(125px, 1fr));
    justify-items: center;
    @media #{$mobileSize} {
      & {
        grid-template-columns: repeat(auto-fill, minmax(70px, 1fr));
      }
    }
    button {
      display: inline-block;
      background-color: #5a9fac;
      font-size: 60px;
      width: 100%;
      height: 125px;
      cursor: pointer;
      border: 2px solid #fff;
      @media #{$mobileSize} {
        & {
          font-size: 30px;
        }
      }
      &.remove-border-right {
        border-right: none;
      }
      &.x {
        color: #5edfff;
      }
      &.o {
        color: red;
      }
    }
  }
  footer {
    p {
      text-align: left;
      margin: {
        top: 50px;
        bottom: 0;
      }
      @media #{$mobileSize} {
        & {
          font-size: 14px;
        }
      }
      a {
        color: #5a9fac;
        font: {
          weight: bold;
        }
      }
    }
  }
}
</style>
