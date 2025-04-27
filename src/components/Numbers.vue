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
  const nums = document.querySelectorAll('.number');
  for(let i = 0; i < nums.length; i++) {
    const num = nums[i].textContent.trim();

    if(number % num === 0) {
      nums[i].classList.add('active');
      console.log('divisor', num);
    }
  }
}

function reset() {
  const nums = document.querySelectorAll('.number');
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
