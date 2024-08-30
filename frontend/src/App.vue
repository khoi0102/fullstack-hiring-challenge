<script lang="ts" setup>
import {ref} from 'vue';

import heroPicker from './components/heroPicker.vue';
import AchillesAvatar from '../public/heros/achilles.png';
import HerculesAvatar from '../public/heros/hercules.png';
import OdysseusAvatar from '../public/heros/odysseus.png';

import type {Hero} from './types';

const heros = ref<Hero[]>([
	{
		name: 'Achilles',
		avatar: AchillesAvatar,
		speed: 10,
		strength: 4,
		intelligence: '6',
	},
	{
		name: 'Odysseus',
		avatar: OdysseusAvatar,
		speed: 6,
		strength: 5,
		intelligence: '9',
	},
	{
		name: 'Hercules',
		avatar: HerculesAvatar,
		speed: 6,
		strength: 10,
		intelligence: '4',
	},
]);
const hero = ref<Hero | null>(null);

let bonus = ref(0);

function doBonus() {
	if (bonus > 5) {
		return alert('Only 5 bonus allowed!');
	}
	//Add an if statement to check if the bonus is exceed 25, if yes then prevent adding more bonus
	if (hero.value) {
		const totalStats = hero.value.speed + hero.value.strength + hero.value.intelligence;
		if (totalStats + bonus.value >= 25) {
			return alert('Cannot exceed total of 25 with bonus!');
		}
	}
	bonus++; // increase bonus!
}

function clearHero() {
	hero.value = null; // Reset the hero to null
}

function handleUpdate(input) {
	console.log('change', input);
	hero.value = input;
}
</script>

<template>
	<div class="flex justify-center items-center min-h-screen">

		<div
			class="
			bg-slate-800
			rounded
			max-w-sm
			grow
			w-full
			text-white
			flex flex-col
			gap-4
			p-4
		"
		>
			<h1>Hero Stats:</h1>
			<div class="flex gap-4">
				<heroPicker
					class="grow"
					v-bind:options="heros"
					v-bind:value="hero"
					v-on:selected="handleUpdate"
				></heroPicker>
				<button
					class="
					border-green-500
					text-sm text-green-400
					border
					p-4
					px-4
					py-2
					rounded
				"
					v-on:click="doBonus"
				>
					BONUS {{ bonus > 0 ? `(${bonus})` : '' }} ✨
				</button>
			</div>
			<div v-if="hero" class="bg-slate-400 text-black rounded flex gap-2">
				<img class="w-52" v-bind:src="hero.avatar">
				<div>
					<h2 class="uppercase text-xs mb-4">
						Hero Summary
					</h2>
					<p>
						<span v-text="hero.name"></span>
					</p>
					<dl>
						<dt class="uppercse text-sm">
							Speed:
						</dt>
						<dd v-text="hero.speed + bonus"></dd>
						<dt class="uppercse text-sm">
							Strength:
						</dt>
						<dd v-text="hero.strength + bonus"></dd>
						<dt class="uppercse text-sm">
							Intelligence:
						</dt>
						<dd v-text="hero.intelligence + bonus"></dd>
					</dl>
					<button
						class="mt-3 px-4 py-2 bg-red-400 text-white rounded hover:bg-red-600"
						v-on:click="clearHero"
					>
						Clear Hero
					</button>
				</div>
			</div>
		</div>
	</div>
</template>
