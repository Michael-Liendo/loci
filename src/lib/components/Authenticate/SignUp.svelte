<script>
	import { newForm } from '@whizzes/svelte-forms';
	import { createEventDispatcher } from 'svelte';
	import * as Yup from 'yup';
	import Button from '../Button.svelte';
	import Input from '../Input.svelte';

	const dispatch = createEventDispatcher();

	const { handleSubmit, values, errors, isSubmitting } = newForm({
		initialValues: {
			name: '',
			lastName: '',
			email: '',
			password: '',
			confirmPassword: ''
		},
		validationSchema: Yup.object({
			name: Yup.string().required(),
			lastName: Yup.string().required(),
			email: Yup.string().email().required(),
			password: Yup.string().min(8, 'Password is too short - should be 8 chars minimum').required(),
			confirmPassword: Yup.string()
				.required()
				.oneOf([Yup.ref('password')], 'password does not match')
		}),
		onSubmit: async (values, helpers) => {
			console.log(values.email, values.password);
		}
	});
</script>

<svelte:head>
	<title>Sign up</title>
</svelte:head>
<div>
	<form on:submit={handleSubmit}>
		<Input
			name="name"
			type="text"
			label="Name"
			placeholder="Joe"
			error={$errors.name}
			bind:value={$values.name}
		/>
		<Input
			name="last-name"
			type="text"
			label="Last name"
			placeholder="Doe"
			error={$errors.lastName}
			bind:value={$values.lastName}
		/>
		<Input
			name="email"
			type="text"
			id="email"
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
		<Input
			name="confirm-password"
			type="password"
			label="Confirm password"
			placeholder="Confirm you password"
			error={$errors.confirmPassword}
			bind:value={$values.confirmPassword}
		/>
		<div class="flex justify-between mt-2">
			<Button disabled={$isSubmitting} type="submit" variant="primary">Create account</Button>
			<Button disabled={$isSubmitting} on:click={() => dispatch('toggleForm')}
				>Login into your account</Button
			>
		</div>
	</form>
</div>
