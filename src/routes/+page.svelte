<script lang="ts">
	import { getPropsObj } from './(previews)/helpers';
	import schemas from './(previews)/schemas';

	// eslint-disable-next-line @typescript-eslint/no-explicit-any
	function getPropsObjForSchema(schema: (typeof schemas)[keyof typeof schemas]): any {
		// eslint-disable-next-line @typescript-eslint/ban-types
		return getPropsObj<{}>(schema.meta);
	}

	$: sortedEntries = Object.entries(schemas).sort((a, b) => {
		return a[1].title.toLowerCase().localeCompare(b[1].title.toLowerCase());
	});
</script>

<div class="wrapper">
	{#each sortedEntries as [identifier, schema]}
		{@const propsObj = getPropsObjForSchema(schema)}
		<div class="flex h-full flex-col gap-2 overflow-hidden">
			<a href={`/${identifier}`} class="flex items-baseline justify-between">
				<h2 class="text-2xl font-bold capitalize text-white">{schema.title}</h2>
				<span class="text-sm text-slate-300">View docs</span></a
			>
			<div class="comp-preview grow place-items-center">
				<svelte:component this={schema.example} {propsObj} />
			</div>
		</div>
	{/each}
</div>

<style lang="postcss">
	.wrapper {
		display: grid;
		padding: theme('spacing.4');
		gap: theme('spacing.8');

		@media screen('md') {
			grid-template-columns: repeat(auto-fill, minmax(500px, 1fr));
			grid-auto-rows: minmax(400px, auto);
		}
	}
</style>
