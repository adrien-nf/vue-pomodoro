<template>
	<section :class="`hero slow-transition ` + (type === POMODORO ? 'is-tomato' : 'is-pepperbell') + ` has-text-light pomodoro-container`">
		<div class="hero-body">
			<Timer
			:currentTimer="currentTimer"
			:elapsedTime="elapsedTime"
			:pomodorosCompleted="pomodorosCompleted"
			:type="type" @skip="$emit('skip')"
			@setStep="(step) => $emit('setStep', step)"
			@toggleTimer="$emit('toggleTimer')"
			:timer="timer"
			/>
			<Tasks :tasks="tasks" @create="createTask" @toggleTask="toggleTask"/>
		</div>
	</section>
</template>

<script>
import Timer from "./Timer.vue";
import Tasks from "./Tasks.vue";
import { POMODORO } from "../consts/types";

export default {
	name: "PomodoroClock",
	components: {
		Timer,
		Tasks,
	},
	props: {
		type: {},
		tasks: {},
		pomodorosCompleted: {},
		currentTimer: {},
		elapsedTime: {},
		timer: {},
	},
	data: () => ({
		POMODORO
	}),
	methods: {
		createTask(taskName) {
			this.$emit("create", taskName);
		},
		toggleTask(index) {
			this.$emit("toggleTask", index)
		}
	}
}
</script>

<style scoped>
.pomodoro-container {
	border-radius: 16px;
}
</style>