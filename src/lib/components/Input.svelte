<script lang="ts">
	export let type: 'text' | 'number' | 'email' | 'password' | 'date' = 'text';
	export let name: string;
	export let id = '';
	export let error: string | null = null;
	export let value: string | number | Date | null = null;
	export let label: string | null = null;
	export let placeholder: string | undefined = undefined;
	export let required = false;
	export let autocomplete: 'true' | 'false' | undefined = undefined;
	export let icon: any = null;

	export let className: string | undefined = undefined;
	export { className as class };

	const handleInput = (event: Event): void => {
		const target = event.target as HTMLInputElement;

		value = type.match(/^(number|range)$/) ? +target.value : target.value;
	};
</script>

<div class="flex flex-col {className}">
	{#if label}
		<label for={name} class:text-red-600={!!error} class="block font-medium text-gray-700 pb-2"
			>{label}</label
		>
	{/if}
	<div class="relative">
		{#if icon}
			<span
				class:text-red-600={!!error}
				class="absolute text-center text-slate-300 bg-transparent rounded text-base items-center justify-center pl-3 py-3"
			>
				<svelte:component this={icon} />
			</span>
		{/if}
		<input
			{name}
			{id}
			{placeholder}
			{required}
			{autocomplete}
			{type}
			{value}
			class:border-red-600={!!error}
			class:pl-10={Boolean(icon)}
			class="text-md border font-semibold px-3 py-2 placeholder-slate-300 text-slate-600 bg-white rounded-lg text-sm outline-none focus:outline-none focus:ring w-full"
			on:change
			on:blur
			on:input={handleInput}
		/>
	</div>
	<p class:opacity-0={!error} class="text-sm pt-1 text-red-600">{error}</p>
</div>
