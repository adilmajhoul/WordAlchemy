<script lang="ts">
	export let form;
</script>

<section class="">
	<div class="mx-auto flex flex-col items-center justify-center px-6 py-8 md:h-screen lg:py-0">
		<div
			class="w-full rounded-lg bg-white shadow-2xl dark:border dark:border-gray-700 dark:bg-gray-800 sm:max-w-md md:mt-0 xl:p-0"
		>
			<div class="space-y-4 p-6 sm:p-8 md:space-y-6">
				<h1
					class="text-xl font-bold leading-tight tracking-tight text-gray-900 dark:text-white md:text-2xl"
				>
					Create and account
				</h1>
				<form class="space-y-4 md:space-y-3" action="?/register" method="POST">
					<label for="email" class="mb-2 block text-sm font-medium text-gray-900 dark:text-white"
						>Your email</label
					>
					<input
						type="email"
						name="email"
						class="block w-full rounded-lg border border-gray-300 bg-gray-50 p-2.5 text-gray-900 focus:border-primary-600 focus:ring-primary-600 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400 dark:focus:border-blue-500 dark:focus:ring-blue-500 sm:text-sm"
						placeholder="name@mail.com"
						required
					/>

					{#if form?.invalidFormat}
						<p class="text-red-600">Invalid email Format</p>
					{/if}

					{#if form?.emailTaken}
						<p class="text-red-600">email is taken.</p>
					{/if}

					{#if form?.success}
						<p class="text-green-600">Account created check your email for confirmation.</p>
					{/if}

					<label for="password" class="mb-2 block text-sm font-medium text-gray-900 dark:text-white"
						>Password</label
					>
					<input
						type="password"
						name="password"
						placeholder="••••••••"
						class="block w-full rounded-lg border border-gray-300 bg-gray-50 p-2.5 text-gray-900 focus:border-primary-600 focus:ring-primary-600 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400 dark:focus:border-blue-500 dark:focus:ring-blue-500 sm:text-sm"
						required
					/>
					<label
						for="confirm-password"
						class="mb-2 block text-sm font-medium text-gray-900 dark:text-white"
						>Confirm password</label
					>
					<input
						type="confirm-password"
						name="confirm-password"
						placeholder="••••••••"
						class="block w-full rounded-lg border border-gray-300 bg-gray-50 p-2.5 text-gray-900 focus:border-primary-600 focus:ring-primary-600 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400 dark:focus:border-blue-500 dark:focus:ring-blue-500 sm:text-sm"
						required=""
					/>

					{#if form?.identicalPasswords}
						<p class="text-red-600">make sure the passwords are identical</p>
					{/if}

					<div class="flex items-start">
						<div class="flex h-5 items-center">
							<input
								id="terms"
								aria-describedby="terms"
								type="checkbox"
								class="focus:ring-3 h-4 w-4 rounded border border-gray-300 bg-gray-50 focus:ring-primary-300 dark:border-gray-600 dark:bg-gray-700 dark:ring-offset-gray-800 dark:focus:ring-primary-600"
								required=""
							/>
						</div>
						<div class="ml-3 text-sm">
							<label for="terms" class="font-light text-gray-500 dark:text-gray-300"
								>I accept the <a
									class="font-medium text-primary-600 hover:underline dark:text-primary-500"
									href="#">Terms and Conditions</a
								></label
							>
						</div>
					</div>

					<button
						type="submit"
						class="w-full rounded-lg bg-primary-600 px-5 py-2.5 text-center text-sm font-medium text-white hover:bg-primary-700 focus:outline-none focus:ring-4 focus:ring-primary-300 dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800"
						>Create an account</button
					>
					<p class="text-sm font-light text-gray-500 dark:text-gray-400">
						Already have an account? <a
							href="/login"
							class="font-medium text-primary-600 hover:underline dark:text-primary-500"
							>Login here</a
						>
					</p>
				</form>
			</div>
		</div>
	</div>
</section>
