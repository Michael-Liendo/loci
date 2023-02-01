<script lang="ts" context="module">
	export type AuthenticationUseCase = 'login' | 'signup';
</script>

<script lang="ts">
	import Card from '../Card.svelte';
	import Login from './Login.svelte';
	import SignUp from './SignUp.svelte';
	import { onMount } from 'svelte';

	export let useCase: AuthenticationUseCase = 'login';

	// fetching a imagen from bing and parse image
	let backgroundUrl: string;

	onMount(async () => {
		const request = await fetch('https://bing.biturl.top/');
		const response = await request.json();
		backgroundUrl = response.url;
	});

	function handleToggleForm() {
		useCase = useCase === 'login' ? 'signup' : 'login';
	}
</script>

<div
	class="flex h-screen bg-slate-900 justify-center items-center"
	style={backgroundUrl && `background-image: url(${backgroundUrl})`}
>
	<Card class="mx-auto w-full sm:w-2/3 md:w-2/6">
		<div class="md:px-3 py-6 z-10">
			<h3 class="text-2xl font-bold text-center mb-4">
				{#if useCase === 'login'}
					Login into your account
				{:else}
					Create an account
				{/if}
			</h3>
			{#if useCase === 'login'}
				<Login on:toggleForm={handleToggleForm} />
			{:else}
				<SignUp on:toggleForm={handleToggleForm} />
			{/if}
		</div>
	</Card>
</div>
