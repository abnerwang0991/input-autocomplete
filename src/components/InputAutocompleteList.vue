<script setup>
import { computed } from 'vue'
const props = defineProps(['inputValue', 'countryList'])

/**
* @param {string} val  被比對的字串
* @param {string} target 輸入的字串
* @return {boolean} 若target順序符合val中的字元返回true，否則返回false
*/
const isContained = (val, target = props.inputValue) => {
	val = val.toLowerCase()
	target = target.toLowerCase()
	
	let i = 0
	let j = 0
	while(i < val.length) {
		if(val[i] === target[j]) {
			j++
			if(j >= target.length) return true
		}
		i++
	}

	return false
}

const autocompleteList = computed(() => (
	props.countryList.filter(val => isContained(val?.name?.common))
))

</script>

<template>
	<div 
		v-if="autocompleteList.length" 
		class="autocomplete-list"
	>
		<div 
			:key="common" 
			v-for="{name: {common}} in autocompleteList"
			class="autocomplete-list__item"
			@click="$emit('updateInputValue', common)"
		>
			{{ common }}
		</div>
	</div>
</template>

<style lang="scss" scoped>
	.autocomplete-list {
		position: absolute;
		top: 40px;
		width: 100%;
		max-height: 30vh;
		overflow: auto;
		border: 1px solid #282828;

		&__item {
			cursor: pointer;
			padding: 8px;

			&:hover{
				background: #f2f2f2;
			}
		}
	}
</style>