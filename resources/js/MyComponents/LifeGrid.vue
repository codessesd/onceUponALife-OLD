<template>
  <!-- Dimensions of the-grid and the blocks need to be precisely calculated so that 52 blocks fit in one row
    currently each block is 7px wide (i.e. width 5px and margin-left 5px),
    therefore 52 blocks = 364px width, the blocks' container has 40px padding, meaning
    that the entire container must have a width of 364px + 40px = 404px
   -->
  <div
    class="backdrop-blur-sm shadow-2xl bg-white/80 rounded-lg pb-5"
    :style="{
      paddingLeft: containerPadding + 'px',
      paddingRight: containerPadding + 'px',
    }"
  >
    <div class="mb-3" :style="{ width: containerWidth + 'px' }">
      <!-- <p class="text-center font-bold text-xl my-3">Once Upon a Life</p> -->
      <div class="flex justify-center mt-3 mb-2">
        <div class="bg-white/40 rounded-lg shadow-2xl">
          <img src="/images/OnceUponALife_text.png" alt="logo" width="150" />
        </div>
      </div>
      <div class="flex justify-center text-center">
        <div
          class="bg-amber-600 h-[9px] w-[10px] mr-1 mt-[8px] shadow-sm"
        ></div>
        <p class="">
          = Number of weeks you have lived if you live to be
          <b>{{ lifeExpectancy }}</b> years
        </p>
      </div>
    </div>

    <div
      id="the-grid"
      class="relative bg-gray-100/0 flex flex-wrap rounded-lg m-auto"
      :style="{ width: containerWidth + 'px' }"
    >
      <div
        class="absolute left-0 top-0 -z-10 h-full flex justify-center items-center"
      >
        <div class="rounded-2xl opacity-20">
          <img src="/images/OnceUponALife_tree_2.png" alt="" />
        </div>
      </div>
      <div
        v-for="(week, index) in weekNumber"
        :key="index"
        class="inline-block"
        :style="{
          marginBottom: '1px',
          height: blockDimensions + 'px',
          width: blockDimensions + 'px',
          marginLeft: '1px',
          marginRight: '1px',
        }"
        :class="{
          ' bg-amber-600 ': index + 1 <= weeksLived,
          ' border border-gray-600 ': index + 1 > weeksLived,
        }"
      ></div>
    </div>
  </div>
</template>
<script setup>
import { ref, onUnmounted } from "vue";

const Props = defineProps({
  weeksLived: {
    type: Number,
    default: 0,
  },
  lifeExpectancy: {
    type: Number,
    default: 73,
  },
});

const weeksInYear = ref(52);
let weekNumber = weeksInYear.value * Props.lifeExpectancy;

// All measurements below are in pixels (px)
let blockDimensions = ref(5);
let blockMarginLeft = 1;
let blockMarginRight = 1;
let containerPadding = ref(20);

let containerWidth =
  (blockDimensions.value + blockMarginLeft + blockMarginRight) *
  weeksInYear.value;

const handleResize = () => {
  blockDimensions.value = window.innerWidth < 400 ? 4 : 5;
  containerPadding.value = window.innerWidth < 400 ? 10 : 20;
  containerWidth =
    (blockDimensions.value + blockMarginLeft + blockMarginRight) *
    weeksInYear.value;
};

window.addEventListener("resize", handleResize);

onUnmounted(() => {
  window.removeEventListener("resize", handleResize);
});
</script>

<style scoped>
:root {
  --dimensions: 5px;
  --paddings: 20px;
}
</style>
