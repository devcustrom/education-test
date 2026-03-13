<template>
	<div class="container mx-auto p-10 gap-5 grid grid-cols-2">
		<div class="relative grid gap-x-2 grid-cols-[auto_auto_1fr]">
			<transition-group name="lift">
				<div
					v-for="item in floors"
					:key="item"
					class="h-10 w-50 bg-amber-300 grid-cols-subgrid items-center grid col-span-3"
				>
					<p class="p-1">Этаж: {{ item }}</p>
					<button
						class="size-10 bg-green-400"
						@click="getLift(item)"
					>
						0
					</button>
				</div>
			</transition-group>
			<div
				class="size-10 right-5 absolute bg-red-800 bottom-[calc((var(--floor)*var(--length)))] translate-y-full transition-all duration-(--duration) grid grid-cols-2 gap-px"
				:style="`
					--floor: ${activeFloor};
					--duration: ${duration}s;
					--length: ${100 / length}%;
				`"
				@transitionend.self="end"
				@transitionstart.self="start"
			>
				<div
					class="size-full bg-black transition-all"
					:class="open ? '-translate-x-full' : ''"
				></div>
				<div
					class="size-full bg-black transition-all"
					:class="open ? 'translate-x-full' : ''"
				></div>
			</div>
		</div>
		<div class="grid gap-4 h-max">
			<div class="grid grid-cols-2 gap-[inherit] max-w-max">
				<button
					class="size-10 bg-amber-700"
					@click="length--"
				>-</button>
				<button
					class="size-10 bg-amber-700"
					@click="length++"
				>+</button>
			</div>
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
			<div class="max-w-max">
				<div
					class="flex flex-col items-center justify-center gap-4 p-4 bg-zinc-800 z-0 transition-opacity duration-200"
				>
					<div class="text-4xl text-yellow-300 font-bold leading-tight">{{ activeFloor }}</div>
					<div class="grid grid-cols-2 gap-2 w-full">
						<button
							v-for="item in floors"
							:key="item"
							class="rounded-full size-7 transition-colors flex items-center justify-center ring-red"
							:class="item === activeFloor ? 'bg-yellow-400 text-black' : 'bg-zinc-600 text-zinc-300'"
							@click.stop="getLift(item)"
						>{{ item }}</button>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts">
	import { computed, ref, watch } from 'vue';

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
		}, 1500)
	}
</script>

<style>
	@reference '../style/index.css';
	.lift-enter-active,
	.lift-leave-active {
		@apply transition-all duration-1000;
	}
	.lift-enter-from,
	.lift-leave-to {
		@apply opacity-0 max-h-0;
	}

</style>