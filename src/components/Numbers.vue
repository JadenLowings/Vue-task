<script setup lang="ts">
import { ref } from 'vue'

// - Removed numbers declaration as it was not needed.
// - Adding ref to change limit on user input.
// - Changed let limit -> const limit to prevent unwanted reassignment.
const limit = ref(100);

function n() {
  let numbers = [];
  // - Changed var to let. Var would be visible outside the
  // for loop and is considered bad practice.
  // - Changed limit -> limit.value to target the data within the object.
  for(let i = 0; i < limit.value; i++) {
    // - Used .push() to manipulate the existing
    // array instead of creating a new one with each iteration.
    // This was to reduce performance cost.
    numbers.push(i);
  }
  return numbers.sort(() => Math.random() - 0.5);
}

function hov(number) {
  // - Converting the given value to an integer to ensure the type.
  const validNumber = parseInt(number);
  // - Adding a check to ensure value is a valid integer before continuing with the function.
  if (isNaN(validNumber)) {
    console.error("Error given value 'number' is not a valid integer.");
    return;
  }
  const nums = document.querySelectorAll('.number');
  for(let i = 0; i < nums.length; i++) {
    // - I used the ? operator to safely access the nested property and
    // return undefined in the case that nums[i].textContent is null.
    // - I used the ?? operator to return an empty string in the case of an issue.
    // This causes parseInt to return NaN triggering my error handling.
    const num = parseInt(nums[i].textContent?.trim() ?? "");
    if (isNaN(num)) {
      console.error("Error when trying to convert nums[i] value to an integer.");
      return;
    }
    // - Added the below condition due to project specifications.
    // 'user hovers over 21, the numbers 1, 3, 7 would be highlighted.'
    if (validNumber === num) continue;
    // - Using validNumber as both validNumber and num are integers.
    // This will help avoid unexpected issues when performing calculations.
    if(validNumber % num === 0) {
      nums[i].classList.add('active');
      // - Removed console.log() so that there are no unnecessary logs.
    }
  }
}

function reset() {
  // - Only selecting elements with both classes .number and .active
  // to reduce the amount of records to iterate through.
  const nums = document.querySelectorAll('.number.active');
  nums.forEach(num => num.classList.remove('active'));
}
</script>

<template>
  <!-- Added additional div to use css positioning instead of line breaks. -->
	<div class="number-container">
      <div class="input-field">
        <!-- Added a heading to help the user quickly identify what the input is used for. -->
        <h3>Limit: </h3>
        <!-- Added the min and max tags to conform with project requirements 'from 1 to 100'. -->
        <input type="number"
               id="user-limit"
               v-model="limit"
               max="100"
               min="1" />
    </div>
    <div>
      <div class="number"
           :id="'number-'+number"
           v-for="number in n()"
           :key="number"
           @mouseover="hov(number)"
           @mouseout="reset">
        {{ number }}
      </div>
    </div>
	</div>
</template>

<style>
/*
- I used flexbox because it allows me to easily position elements
and resizes automatically allowing a responsive design.
*/
.number-container {
  display: flex;
  flex-direction: column;
}
.input-field {
  display: flex;
  align-items: center;
  justify-content: center;
}

.input-field h3 {
  margin-right: 10px;
}

.input-field #user-limit {
  padding: 10px;
  border: transparent;
  border-radius: 10px;
}

/*
- Changed the width, border-radius and color to
display the data in a more user friendly manner.
*/
.number {
  display: inline-block;
	padding: 5px;
  min-width: 30px;
  border-radius: 10px;
	background-color: lightgrey;
  color: black;
	margin: 5px;
}

/*
- Changed the background-color to look better with the app background.
- Added bold font and transformed scale to make it easier for the user to
quickly identify highlighted data.
*/
.active {
	background-color: #91E5F6;
  font-weight: bold;
  transform: scale(1.2);
}
</style>
