<script context="module">
	export async function load({ fetch }) {
		const res = await fetch('/data/apps.json');

		if (res.ok) {
			const apps = await res.json();
			apps.sort((a, b) =>
				a.canonical_app_name.toLowerCase() > b.canonical_app_name.toLowerCase() ? 1 : -1
			);
			return {
				props: {
					apps
				}
			};
		}

		const { message } = await res.json();
		return { error: new Error(message) };
	}
</script>

<script>
	export let apps;

</script>

<svelte:head>
	<title>Glean Dictionary</title>
</svelte:head>

<div class="mzp-c-emphasis-box mzp-t-dark hero-box">
	<h5>
		The Glean Dictionary documents the data collected by Mozilla projects that use <a
			href="https://mozilla.github.io/glean/">Glean</a
		>.
	</h5>
	<p>
		Select a project to browse its data catalog. If you have questions, please ask in the <a
			href="https://chat.mozilla.org/#/room/#glean-dictionary:mozilla.org">#glean-dictionary</a
		> channel on Mozilla's instance of Matrix.
	</p>
</div>

{#if apps}
	<div class="mzp-c-emphasis-box">
		<div class="app-list">
			{#each apps as app}
				{#if !app.deprecated}
					<div class="mzp-c-card mzp-c-card-extra-small has-aspect-3-2">
						<a class="mzp-c-card-block-link" href={app.app_name} id="media-block">
							<div class="mzp-c-card-content">
								<h6 class="mzp-c-card-title">{app.canonical_app_name}</h6>
								<p>
									{app.app_description}
								</p>
							</div>
						</a>
					</div>
				{/if}
			{/each}
		</div>
	</div>
{/if}

<style>
	.hero-box {
		text-align: center;
	}

	.app-list {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
		grid-gap: $spacing-md;
	}
</style>
