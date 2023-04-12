<template>
  <div class="chess-board max-w-[640px] mx-auto">
    <div v-for="(i, row) in 8" :key="i" class="flex justify-center">
      <div v-for="(k, col) in 8" :key="k" :class="getSquareClass(row, col)">
        <cell
          :value="board[row][col]"
          @click="handleClick(row, col)"
          :isHighLight="checkHighLight(row, col)"
          :isDarkSquare="checkDarkSquare(row, col)"
        />
      </div>
    </div>
    {{ selectPieceXY }}
  </div>
</template>

<script>
import Cell from "./Cell.vue";
export default {
  components: { Cell },
  name: "Board",
  data() {
    return {
      // Rook - xe
      // Knight - mã
      // Bishop - tượng
      // Queen - Hậu
      // King - vua
      // Pawn - tốt
      board: [
        ["R", "N", "B", "Q", "K", "B", "N", "R"],
        ["P", "P", "P", "P", "P", "P", "P", "P"],
        ["", "", "", "", "", "", "", ""],
        ["", "", "", "", "", "", "", ""],
        ["", "", "", "", "", "", "", ""],
        ["", "", "", "", "", "", "", ""],
        ["p", "p", "p", "p", "p", "p", "p", "p"],
        ["r", "n", "b", "q", "k", "b", "n", "r"],
      ],
      selectPieceXY: null,
      selectRow: null,
      selectCol: null,
      selectPiecee: null,
      validMoves: [],
      turn: "white",
    };
  },

  // Kiểm tra click ra ngoài bàn cờ
  created() {
    window.addEventListener("click", (e) => {
      const boardEl = this.$el;
      if (!boardEl.contains(e.target)) {
        this.selectPieceXY = null;
      }
    });
  },

  watch: {
    selectPieceXY(value) {
      if (value === null) {
        this.validMoves = [];
        return;
      }
      const [fromRow, fromCol] = value;
      const piece = this.board[fromRow][fromCol];

      if (piece === "p" && this.turn === "white") {
        if (fromRow === 6) {
          this.validMoves.push([4, fromCol], [5, fromCol]);
        } else if (fromCol >= 0 && fromRow >= 0) {
          const pawnsWhite1 = this.board[fromRow - 1][fromCol - 1];
          const isPawnWhite1 = pawnsWhite1 === pawnsWhite1.toLowerCase();
          if (pawnsWhite1 !== "" && !isPawnWhite1) {
            this.validMoves.push([fromRow - 1, fromCol - 1]);
          }

          const pawnsWhite2 = this.board[fromRow - 1][fromCol + 1];
          const isPawnWhite2 = pawnsWhite2 === pawnsWhite2.toLowerCase();
          if (pawnsWhite2 !== "" && !isPawnWhite2) {
            this.validMoves.push([fromRow - 1, fromCol + 1]);
          }

          if (this.board[fromRow - 1][fromCol] === "") {
            this.validMoves.push([fromRow - 1, fromCol]);
          }
        }
      }

      if (piece === "P" && this.turn === "black") {
        if (fromRow === 1) {
          this.validMoves.push([2, fromCol], [3, fromCol]);
        } else if (fromCol <= 7 && fromRow <= 7) {
          const pawnsBlack1 = this.board[fromRow + 1][fromCol + 1];
          const isPawnBlack1 = pawnsBlack1 === pawnsBlack1.toUpperCase();
          if (pawnsBlack1 !== "" && !isPawnBlack1) {
            this.validMoves.push([fromRow + 1, fromCol + 1]);
          }

          const pawnsBlack2 = this.board[fromRow + 1][fromCol - 1];
          const isPawnBlack2 = pawnsBlack2 === pawnsBlack2.toUpperCase();
          if (pawnsBlack2 !== "" && !isPawnBlack2) {
            this.validMoves.push([fromRow + 1, fromCol - 1]);
          }

          if (this.board[fromRow + 1][fromCol] === "") {
            this.validMoves.push([fromRow + 1, fromCol]);
          }
        }
      }
    },
  },

  methods: {
    // style màu cho các ô
    getSquareClass(row, col) {
      return [
        "w-20",
        "h-20",
        "flex-shrink-0",
        "border",
        this.selectPieceXY &&
        row === this.selectPieceXY[0] &&
        col === this.selectPieceXY[1]
          ? "selected"
          : "",
      ];
    },

    // Kiểm tra loại quân
    getPieceClass(piece) {
      const isBlack = piece === piece.toUpperCase();
      return [
        "chess-piece",
        isBlack ? "black" : "white",
        `icon-${piece.toLowerCase()}`,
      ];
    },

    checkDarkSquare(row, col) {
      return (row + col) % 2 !== 0;
    },

    checkHighLight(row, col) {
      return this.validMoves.some((el) => {
        return row === el[0] && col === el[1];
      });
    },

    handleClick(row, col) {
      if (this.selectPieceXY === null) {
        if (this.board[row][col] !== "") {
          this.selectPieceXY = [row, col];
        }
      } else {
        const [fromRow, fromCol] = this.selectPieceXY;
        const piece = this.board[fromRow][fromCol];
        const isValidMove = this.validMoves.some(
          (el) => row === el[0] && col === el[1]
        );
        if (isValidMove) {
          this.board[row][col] = piece;
          this.board[fromRow][fromCol] = "";
          if (this.turn === "white") {
            this.turn = "black";
          } else {
            this.turn = "white";
          }
        }
        this.selectPieceXY = null;
      }
    },
  },
};
</script>

<style scoped>
.selected > div {
  @apply bg-yellow-200;
}
</style>
