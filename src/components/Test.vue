<template>
	<div class="container mx-auto p-10 grid grid-cols-2">
		<div class="relative">
			<div
				v-for="item in floors"
				:key="item"
				class="h-10 w-50 bg-amber-300"
			>
				Этаж: {{ item }}
				<button
					class="size-10 bg-green-400"
					@click="getLift(item)"
				>
					0
				</button>
			</div>
			<div
				class="size-10 left-35 absolute bg-red-800 bottom-[calc((var(--floor)*var(--length)))] translate-y-full transition-all duration-(--duration) grid grid-cols-2 gap-px"
				:style="`
					--floor: ${activeFloor};
					--duration: ${duration}s;
					--length: ${100 / length}%;
				`"
				@transitionend="end"
				@transitionstart="start"
			>
				<div
					class="size-full bg-black transition-al"
					:class="open ? '-translate-x-full' : ''"
				></div>
				<div
					class="size-full bg-black transition-al"
					:class="open ? 'translate-x-full' : ''"
				></div>
			</div>
		</div>
		<div class="grid gap-4">
			<button
				class="size-10 bg-amber-700"
				@click="length--"
			>-</button>
			<button
				class="size-10 bg-amber-700"
				@click="length++"
			>+</button>
			<button
				class="h-10 bg-amber-700"
				@click="activeFloor++"
			>Вверх</button>
			<button
				class="h-10 bg-amber-700"
				@click="activeFloor--"
			>Вниз</button>
			
			<button
				class="h-10 bg-amber-700"
				@click="open = !open"
			>Открыть двери</button>
		</div>
	</div>
</template>

<script setup lang="ts">
	import { computed, nextTick, ref, watch } from 'vue';

	const length = ref(9)	
	const floors = computed<number[]>(() => Array.from({ length: length.value }, (_, i) => i + 1).reverse())

	const activeFloor = ref<number>(5)
	const duration = ref(1)
	const open = ref(false)

	watch(activeFloor, (value, oldValue) => {
		duration.value = Math.abs(value - oldValue)
	})

	const getLift = (floor: number) => {
		if(!open.value) {
			activeFloor.value = floor
		}
	}

	const start = () => {
		
	}

	const end = async () => {
		open.value = true
		setTimeout(() => {
			open.value = false
		}, 500)
	}
</script>