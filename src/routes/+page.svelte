<script lang="ts">
	import Centerbox from '$lib/components/Centerbox.svelte';
	import { onMount } from 'svelte';

	let url = '';
	let copied = false;

	const copyUrl = () => {
		navigator.clipboard.writeText(url);
		copied = true;
		setTimeout(() => (copied = false), 2000);
	};

	let links = [
		{
			href: 'https://crates.io/crates/shuffler',
			label: 'My crate, Shuffler, on crates.io',
			key: 0
		},
		{
			href: 'https://developer.spotify.com/documentation/web-api/tutorials/code-flow',
			label: "Spotify's API docs on authentication and callbacks",
			key: 1
		}
	];

	onMount(() => {
		url = window.location.href;
	});
</script>

<Centerbox>
	{#if !url.includes('?code=')}
		<div class="max-w-xl wrap-normal">
			<p class="text-red-500">
				There is no point in opening this site manually. You need to get the callback URL from
				Spotify auth. The following links may be useful:
			</p>
			<ul class="list-disc pl-5 text-sm text-gray-300">
				{#each links as link (link.key)}
					<li>
						<!-- eslint-disable-next-line svelte/no-navigation-without-resolve -->
						<a href={link.href} class="text-blue-300 hover:underline"> {link.label} </a>
					</li>
				{/each}
			</ul>
		</div>
	{:else}
		<div class="flex max-w-xl flex-col rounded-lg bg-zinc-700 p-8 shadow-lg">
			<h1 class="text-2xl font-bold">Shuffler Spofity authentication</h1>
			<p>Click below to copy the page url. Then, paste it back in your terminal.</p>
			<button
				class="group relative mt-2 max-w-full rounded bg-zinc-800 px-2 py-3 text-left text-sm hover:bg-zinc-900"
				on:click={copyUrl}
			>
				<span
					class="absolute right-2 bottom-2 text-zinc-500 transition-colors group-hover:text-white"
				>
					{#if copied}
						<span class="text-xs font-medium text-green-400">Copied ✓</span>
					{:else}
						<svg
							xmlns="http://www.w3.org/2000/svg"
							class="h-4 w-4"
							fill="none"
							viewBox="0 0 24 24"
							stroke="currentColor"
							stroke-width="2"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"
							/>
						</svg>
					{/if}
				</span>

				<pre class="pr-12 break-all whitespace-pre-wrap">{url}</pre>
			</button>
		</div>
	{/if}
</Centerbox>
