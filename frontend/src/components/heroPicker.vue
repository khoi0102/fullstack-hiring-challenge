<template>
	<div class="relative">
		<span
			class="
				block
				w-32
				px-4
				py-2
				text-sm
				font-medium
				text-white
				bg-indigo-600
				rounded-lg
				hover:bg-indigo-700
				focus:outline-none
				focus-visible:ring
				focus-visible:ring-indigo-500
				focus-visible:ring-opacity-75
			"
			v-on:click="toggleMenu"
		>
			<span v-if="!value">Select a Hero</span>
			<span v-else v-text="`Selected: ${value?.name}`"></span>
		</span>
		<ul
			v-show="isOpen"
			class="
				absolute
				right-0
				z-10
				w-32
				mt-2
				origin-top-right
				bg-slate-800
				border border-slate-500
				rounded-md
				shadow-lg
			"
		>
			<li v-for="option in options" v-bind:key="option.name" class="py-1">
				<a
					href="#"
					class="
						flex
						items-center
						px-4
						py-2
						text-sm text-slate-400
						hover:bg-slate-700
						gap-2
					"
					v-on:click="setOption(option)"
				>
					<img
						v-bind:src="option.avatar"
						alt="avatar"
						class="w-6 h-6 round-full ob aspect-square rounded"
					>
					<span v-text="option.name"></span>
				</a>
			</li>
		</ul>
	</div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

import type { Hero } from '../types';
import type { PropType } from 'vue';
const props = defineProps({
	value: Object as PropType<Hero | null>,
	options: Array as PropType<Hero[]>,
});

const emit = defineEmits(['selected']);
const isOpen = ref(false);

function toggleMenu() {
	isOpen.value = !isOpen.value;
}
// so after doing some research I can see that it bad practice to mutate props directly
/*function setOption(input) {
	props.value = input;
	emit('selected', input);
}*/
function setOption(input) {
	emit('selected', input); // Only emit the event
	//To fix the issue where the dropdown menu remains open after a hero is selected, we need to close the dropdown after selecting a hero.
	isOpen.value = false;
}

</script>
