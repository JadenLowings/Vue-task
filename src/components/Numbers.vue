<script setup lang="ts">
import { ref } from 'vue'

// - Removed numbers declaration as it was not needed.
// - Adding ref to change limit on user input.
let limit = ref(100);

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

    // - Using validNumber as both validNumber and num are integers.
    // This will help avoid unexpected issues when performing calculations.
    if(validNumber % num === 0) {
      nums[i].classList.add('active');
      console.log('divisor', num);
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
	<div>
		<input type="number" v-model="limit" /><br /><br />
		<div class="number"
			:id="'number-'+number"
			v-for="number in n()"
			:key="number"
			@mouseover="hov(number)"
			@mouseout="reset"
		>
			{{ number }}
		</div>
	</div>
</template>

<style>
.number {
	display: inline-block;
	padding: 5px;
	background-color: lightgrey;
  color: black;
	margin: 5px;
}

.active {
	background-color: red;
}
</style>
