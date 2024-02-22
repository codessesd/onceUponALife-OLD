<template>
  <div>
    <div class="w-full flex justify-center mt-10">
      <div>
        <img src="/images/logo2.png" width="300px" alt="logo" />
      </div>
    </div>

    <div class="m-auto grid justify-items-center px-4 max-w-[800px]">
      <!-- information panel -->
      <div>
        <p class="text-justify">
          What if you can get a bird's eye view of your life? What if you could
          see how far you've come and how far you have to go?
        </p>
      </div>
      <div id="information-panel" class="w-full max-w-[450px] mb-10 mt-10">
        <div class="flex-auto w-full">
          <InputGroup
            label="Enter your Date of Birth"
            :subtext="subtextAge"
            :value="yearsLived"
          >
            <Calendar
              v-model="dateOfBirth"
              showIcon
              inputId="buttondisplay"
              dateFormat="dd/mm/yy"
              class="w-full"
            >
            </Calendar>
          </InputGroup>

          <InputGroup
            label="Life expectancy in"
            :subtext="subtextExpectancy"
            :value="lifeExpectancy"
          >
            <Dropdown
              v-model="selectedCountry"
              :options="countries"
              placeholder="Select a country"
              class="w-full"
            >
            </Dropdown>
          </InputGroup>
          <div class="flex w-full justify-end">
            <Button @click="toggleGrid" class="px-16 mt-10">Go</Button>
          </div>
          <div
            class="hidden mt-10 shadow-xl border border-gray-50 p-2 rounded-lg"
          >
            <p class="text-sm w-full text-justify text-gray-400">
              Each tiny box represents one week a person’s life. Each row of
              boxes (52 boxes) represents one year of your life. The global life
              expectancy is 73 years, therefore, the would be 73 rows in the
              grid below and if you are 36 years, you have lived about half of
              those boxes. Put in your date of birth to get a perspective of how
              my weeks you have lived and how many weeks you may have left.
            </p>
          </div>
        </div>
      </div>

      <!-- <p class="font-bold text-lg">Life Grid</p> -->
      <div
        v-if="showGrid"
        @click="toggleGrid"
        class="fixed top-0 left-0 w-full h-full flex justify-center items-center bg-black/30 z-10"
      ></div>
      <Transition>
        <LifeGrid
          v-if="showGrid"
          :weeksLived="weeksLived"
          :lifeExpectancy="lifeExpectancy"
          class="fixed top-0 z-20"
        ></LifeGrid>
      </Transition>
    </div>
  </div>
</template>
<script setup>
import { computed, ref, watch } from "vue";
import Calendar from "primevue/calendar";
import Button from "primevue/button";
import Dropdown from "primevue/dropdown";
import InputGroup from "@/MyComponents/InputGroup.vue";
import LifeGrid from "@/MyComponents/LifeGrid.vue";

let showGrid = ref(false);
const toggleGrid = () => {
  showGrid.value = !showGrid.value;
};

const countriesObj = [
  { country: "Rest of the world", value: 73 },
  { country: "South Africa", value: 65 },
  { country: "United States", value: 76 },
  { country: "Hong Kong", value: 80 },
];

let countries = [];
for (let i = 0; i < countriesObj.length; i++) {
  countries.push(countriesObj[i].country);
}

let subtextExpectancy = ref("The Global Life expectancy is");
let lifeExpectancy = ref(countriesObj[1].value);
let selectedCountry = ref(countries[1]);
watch(
  () => selectedCountry.value,
  (newVal) => {
    if (newVal === countries[0])
      subtextExpectancy.value = "The Global Life expectancy is";
    else subtextExpectancy.value = "The Life expectancy in " + newVal + " is";

    lifeExpectancy.value = countriesObj.find(
      (country) => country.country === newVal
    ).value;
  }
);

let subtextAge = ref("Your age will show here");
let weeksLived = ref(0);
let yearsLived = ref(0);
let dateOfBirth = ref(new Date());
watch(
  () => dateOfBirth.value,
  (newVal) => {
    const userDOB = new Date(newVal);
    const today = new Date();
    const dateDiffMilliseconds = today - userDOB;
    weeksLived.value = Math.floor(
      dateDiffMilliseconds / (1000 * 60 * 60 * 24 * 7)
    );
    yearsLived.value = Math.floor(weeksLived.value / 52);
  }
);
</script>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: top 0.5s ease, opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  top: 300px;
  opacity: 0;
}
</style>
