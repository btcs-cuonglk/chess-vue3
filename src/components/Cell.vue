<template>
  <div class="w-full h-full block" :class="getBackgroundColor()">
    <!-- black -->
    <img v-if="value === 'R'" src="../images/Chess_rdt45.svg" />

    <img v-if="value === 'N'" src="../images/Chess_ndt45.svg" />

    <img v-if="value === 'B'" src="../images/Chess_bdt45.svg" />

    <img v-if="value === 'Q'" src="../images/Chess_qdt45.svg" />

    <img v-if="value === 'K'" src="../images/Chess_kdt45.svg" />

    <img v-if="value === 'P'" src="../images/Chess_pdt45.svg" />

    <!-- white -->
    <img v-if="value === 'r'" src="../images/Chess_rlt45.svg" />

    <img v-if="value === 'n'" src="../images/Chess_nlt45.svg" />

    <img v-if="value === 'b'" src="../images/Chess_blt45.svg" />

    <img v-if="value === 'q'" src="../images/Chess_qlt45.svg" />

    <img v-if="value === 'k'" src="../images/Chess_klt45.svg" />

    <img v-if="value === 'p'" src="../images/Chess_plt45.svg" />
  </div>
</template>

<script>
export default {
  name: "Cell",
  props: {
    value: {
      type: String,
      default: "",
    },
    isHighLight: {
      type: Boolean,
      required: true,
    },
    isDarkSquare: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    getBackgroundColor() {
      if (this.isHighLight) {
        return "bg-red-700";
      }
      if (this.isDarkSquare) {
        return "bg-rose-50";
      } else {
        return "bg-yellow-700";
      }
    },
  },
};
</script>

<style scoped>
img {
  width: 100%;
}
</style>
>
