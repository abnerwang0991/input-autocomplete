<script setup>
import InputAutocompleteList from './InputAutocompleteList.vue';

import { ref, onMounted } from 'vue'
import axios from 'axios'

const countryList = ref([])
const getCountryList = async() => {
  try {
    const { data } = await axios({
		method: 'get',
		url: 'https://restcountries.com/v3.1/all?fields=name'
    })

    if(data) {
    	countryList.value = data
    }
  } catch(e) {
	console.log(e)
  }
}

const inputValue = ref('')
const updateInputValue = val => inputValue.value = val

onMounted(() => {
  	getCountryList()
})

</script>

<template>
	<div class="field">
		<input 
			v-model="inputValue"
			type="search"
		/>
		<InputAutocompleteList 
			:input-value="inputValue" 
			:country-list="countryList"
			@updateInputValue="updateInputValue"
		/>
	</div>
</template>

<style lang="scss" scoped>
	.field {
		position: relative;
		display: flex;

		input[type="search"] {
			width: 320px;
			height: 40px;
			font-size: 15px;
		}
	}
</style>