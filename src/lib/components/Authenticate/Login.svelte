<script>
	import { createEventDispatcher } from 'svelte';
	import { newForm } from '@whizzes/svelte-forms';
	import * as Yup from 'yup';
	import Button from '../Button.svelte';
	import Input from '../Input.svelte';

	const dispatch = createEventDispatcher();

	const { handleSubmit, values, errors, isSubmitting } = newForm({
		initialValues: {
			email: '',
			password: ''
		},
		validationSchema: Yup.object({
			email: Yup.string().email().required(),
			password: Yup.string().required()
		}),
		onSubmit: async (values, helpers) => {
			console.log(values.email, values.password);
		}
	});
</script>

<svelte:head>
	<title>Login</title>
</svelte:head>

<div>
	<form on:submit={handleSubmit}>
		<Input
			name="email"
			type="text"
			id="email"
			autocomplete="true"
			label="Email"
			placeholder="user@example.com"
			error={$errors.email}
			bind:value={$values.email}
		/>
		<Input
			type="password"
			name="password"
			id="password"
			label="Password"
			placeholder="Enter you password"
			error={$errors.password}
			bind:value={$values.password}
		/>
		<div class="flex justify-between mt-2">
			<Button disabled={$isSubmitting} type="submit" variant="primary">Login</Button>
			<Button disabled={$isSubmitting} on:click={() => dispatch('toggleForm')}
				>Create an account</Button
			>
		</div>
	</form>
</div>
