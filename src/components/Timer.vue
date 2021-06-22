<template>
	<div class="columns">
		<div class="column">
			<div class="columns">
				<div class="column">
					<div class="tabs is-centered">
						<ul>
							<li :class="type === POMODORO ? 'is-active' : ''"><a @click="() => $emit('setStep', POMODORO)">Pomodoro</a></li>
							<li :class="type === TINY_PAUSE ? 'is-active' : ''"><a @click="() => $emit('setStep', TINY_PAUSE)">Short Pause</a></li>
							<li :class="type === BIG_PAUSE ? 'is-active' : ''"><a @click="() => $emit('setStep', BIG_PAUSE)">Long Pause</a></li>
						</ul>
					</div>
				</div>
			</div>
			<h1 class="timer-time">{{timeLeft}}</h1>
			<div class="columns">
				<div class="column" v-for="(tomato, index) of (new Array(4))" :key="index">
					<div v-show="index >= pomodorosCompleted">
						<svg width="100%" height="100%">       
							<image xlink:href="tomato.svg" width="100%" height="100%"/>    
						</svg>
					</div>
					<div v-show="index < pomodorosCompleted">
						<svg width="100%" height="100%">       
							<image xlink:href="bell-pepper.svg" width="100%" height="100%"/>    
						</svg>
					</div>
				</div>
			</div>
			<button class="button is-fullwidth" @click="$emit('skip')">Skip</button>
		</div>
	</div>
</template>

<script>
import { BIG_PAUSE, POMODORO, TINY_PAUSE } from '../consts/types'
export default {
	props: {
		type: {},
		pomodorosCompleted: {},
		currentTimer: {},
		elapsedTime: {},
	},
	data: () => ({
		POMODORO,
		BIG_PAUSE,
		TINY_PAUSE
	}),
	computed: {
		title() {
			let v = null;
			switch(this.type) {
				case POMODORO:
					v = "Pomodoro"
					break;
				case BIG_PAUSE:
					v = "Big Pause"
					break;
				case TINY_PAUSE:
					v = "Short Pause"
					break;
			}
			return v;
		},
		timeLeft() {
			let mins = ~~((this.currentTimer-this.elapsedTime)/60);
			let s = this.currentTimer - this.elapsedTime - (mins*60);
			return `${mins.toString().padStart(2, 0)}:${s.toString().padStart(2, 0)}`;
		}
	}
}
</script>

<style>
	.timer-time {
		font-size: 5em;
		text-align: center;
	}
</style>