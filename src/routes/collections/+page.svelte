<script>
	import axios from 'axios';
	import { goto } from '$app/navigation';
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';

	export let data;

	const { user, myCollections } = data;
	console.log('🚀 ~ myCollections:', myCollections);
	console.log('🚀 ~ myCollections words:', myCollections[0].words);

	let collections = myCollections;
	let collectionName = '';
	let isCreatingCollection = false;

	let newWordName = '';

	const createCollection = async () => {
		// Create a new collection on the server
		try {
			const response = await fetch(`/api/collections`, {
				method: 'POST',
				body: JSON.stringify({ name: collectionName })
			});

			if (response.ok) {
				const newCollection = await response.json();

				collections = [
					...collections,
					{ ...newCollection, isCreatingNewWord: false, isCollectionWordsOpen: false }
				];
				collectionName = ''; // Clear the input field
			} else {
				// Handle error if needed
				console.error('Failed to create collection');
			}
		} catch (error) {
			console.log(error);
		}
	};
	/*
	const addWord = async (collectionID) => {
		// Create a new collection on the server

		console.log('what is words', collections);
		try {
			const response = await fetch(`/api/words`, {
				method: 'POST',
				body: JSON.stringify({ name: newWordName, id: collectionID })
			});

			if (response.ok) {
				const newCollection = await response.json();

				collections = [
					...collections,
					{ ...newCollection, isCreatingNewWord: false, isCollectionWordsOpen: false }
				];
				newWordName = '';
			} else {
				// Handle error if needed
				console.error('Failed to create collection');
			}
		} catch (error) {
			console.log(error);
		}
	};

	*/

	const addWord = async (collectionID, collectionIndex) => {
		try {
			const response = await fetch(`/api/words`, {
				method: 'POST',
				body: JSON.stringify({ name: newWordName, id: collectionID })
			});

			if (response.ok) {
				const newWord = await response.json();
				console.log('🚀 ~ addWord ~ newWord:', newWord);

				newWord.isCollectionWordsOpen = true;
				collections[collectionIndex] = newWord;
				collections = [...collections];

				newWordName = '';
			} else {
				console.error('Failed to create word');
			}
		} catch (error) {
			console.log(error);
		}
	};

	const deleteCollection = async (collectionID) => {
		// Create a new collection on the server
		try {
			const response = await fetch(`/api/collections`, {
				method: 'DELETE',
				body: JSON.stringify({ id: collectionID })
			});

			if (response.ok) {
				const newCollection = await response.json();

				collections = collections.filter((collection) => collection.id != collectionID);
				collectionName = ''; // Clear the input field
			} else {
				// Handle error if needed
				console.error('Failed to create collection');
			}
		} catch (error) {
			console.log(error);
		}
	};

	// async function fetchCollection(collectionID, index, isOpen) {
	// 	try {
	// 		const response = await fetch(`/api/collection`, {
	// 			method: 'POST',
	// 			body: JSON.stringify({ collectionID })
	// 		});

	// 		if (response.ok) {
	// 			const updatedCollection = await response.json();
	// 			console.log('🚀 ~ fetchCollection ~ updatedCollection:', updatedCollection);
	// 			console.log('🚀 ~ fetchCollection ~ updatedCollection words:', updatedCollection.words);
	// 			collections[index] = updatedCollection;
	// 			collections = [...collections];
	// 		} else {
	// 			console.error('Failed to fetch collection');
	// 		}
	// 	} catch (error) {
	// 		console.log(error);
	// 	}

	// 	return isOpen;
	// }

	// async function fetchCollections() {
	// try {
	// const response = await fetch(`/api/collections`, {
	// method: 'GET'
	// });
	//
	// if (response.ok) {
	// const updatedCollections = await response.json();
	// console.log('🚀 ~ fetchCollections ~ updatedCollections:', updatedCollections);
	// collections = [...updatedCollections];
	// } else {
	// console.error('Failed to fetch collection');
	// }
	// } catch (error) {
	// console.log(error);
	// }
	// }
</script>

<div class="space-y-4 my-3 mx-4 flex flex-col justify-center">
	{#if !isCreatingCollection}
		<button
			on:click={() => (isCreatingCollection = true)}
			class="mx-auto space-x-3 flex items-center justify-center rounded-lg h-10 w-1/2 border-2 border-blue-400"
			type="submit">
			<span>Create Collection</span>
			<span class="text-3xl">+</span>
		</button>
	{/if}

	{#if isCreatingCollection}
		<form
			class="bg-gray-400/30 rounded-lg py-3 space-y-3 flex flex-col"
			on:submit|preventDefault={createCollection}>
			<input
				class=" border-2 border-blue-400 mx-3 px-3 h-11 rounded rouned-lg text-gray-900"
				type="text"
				id="collectionName"
				bind:value={collectionName}
				required
				placeholder="Create Collection ..." />

			<div class="space-y-3 w-full flex flex-col items-center">
				<button
					class="h-10 w-1/2 text-white hover:text-gray-900 dark:text-gray-900 dark:hover:text-white rounded-lg px-2 border-2 border-blue-400 bg-blue-400 hover:bg-transparent"
					type="submit">Create Collection</button>
				<button
					on:click={() => (isCreatingCollection = false)}
					class="h-10 w-1/2 border-blue-400 hover:bg-blue-400 hover:text-white dark:hover:bg-blue-400 dark:hover:text-gray-900 dark:hover:border-blue-400 px-2 rounded-lg border-2 flex justify-center items-center"
					type="submit">
					Cancel
				</button>
			</div>
		</form>
	{/if}

	{#if collections.length > 0}
		<ul class="space-y-4">
			{#each collections as collection, index}
				<button
					key={index}
					class="flex justify-between items-center px-3 h-14 rounded-lg w-full border-2 border-blue-400"
					on:click={async () => {
						// if (collection.isCollectionWordsOpen) {
						// 	await fetchCollection(collection.id, index);
						// }

						collection.isCollectionWordsOpen = !collection.isCollectionWordsOpen;
					}}>
					<span>{collection.name}</span>

					<div class="space-x-4 flex items-center">
						<button
							on:click={() => (collection.isCreatingNewWord = !collection.isCreatingNewWord)}
							class="border-blue-400 hover:bg-blue-400 hover:text-white dark:hover:bg-blue-400 dark:hover:text-gray-900 dark:hover:border-blue-400 px-2 rounded-lg border-2 flex items-center"
							>add word</button>
						<!--
							<button
							class="text-white hover:text-gray-900 dark:text-gray-900 dark:hover:text-white rounded-lg px-2 border-2 border-blue-400 bg-blue-400 hover:bg-transparent"
							on:click={() =>
								(collection.isCollectionWordsOpen = !collection.isCollectionWordsOpen)}
							>{collection.isCollectionWordsOpen ? 'close' : 'expand'}</button>
							-->
					</div>
				</button>

				{#if collection.isCreatingNewWord}
					<form
						class="bg-gray-400/30 rounded-lg py-3 space-y-3 flex flex-col"
						on:submit|preventDefault={addWord(collection.id, index)}>
						<input
							class=" border-2 border-blue-400 mx-3 px-3 h-11 rounded rouned-lg text-gray-900"
							type="text"
							bind:value={newWordName}
							required
							placeholder="Add New Word ..." />

						<div class="space-y-3 w-full flex flex-col items-center">
							<button
								class="h-10 w-1/2 text-white hover:text-gray-900 dark:text-gray-900 dark:hover:text-white rounded-lg px-2 border-2 border-blue-400 bg-blue-400 hover:bg-transparent"
								type="submit">Add Word</button>
							<button
								on:click={() => (collection.isCreatingNewWord = !collection.isCreatingNewWord)}
								class="h-10 w-1/2 border-blue-400 hover:bg-blue-400 hover:text-white dark:hover:bg-blue-400 dark:hover:text-gray-900 dark:hover:border-blue-400 px-2 rounded-lg border-2 flex justify-center items-center"
								type="submit">
								Cancel
							</button>
						</div>
					</form>
				{/if}

				{#if collection.isCollectionWordsOpen}
					<ul class="p-3 space-y-3 px-3 rounded-lg">
						{#each collection.words as word, index}
							<li
								class="px-2 items-center flex justify-between bg-gray-400/30 h-10 rounded-lg"
								key={index}>
								{word.text}

								<div class="space-x-2">
									<button class="hover:text-gray-500 dark:hover:text-gray-950"
										><a href={`/Definition/${word.text}`}>Definition</a></button>
								</div>
							</li>
						{/each}
					</ul>

					<button
						on:click={deleteCollection(collection.id)}
						class="px-3 flex justify-end w-full text-red-600">Delete Collection</button>
				{/if}
			{/each}
		</ul>
	{:else}
		<p>No collections found.</p>
	{/if}
</div>
