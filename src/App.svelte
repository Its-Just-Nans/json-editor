<script>
	import RenderStruct from "./RenderStruct.svelte";
	let files;
	$: reactiveParsed = JSON.stringify(parsed, null, 4);
	let parsed = null;
	const parsefiles = async () => {
		try {
			if (files) {
				const parseFile = (fileToParse) => {
					return new Promise((resolve, reject) => {
						const reader = new FileReader();
						reader.onload = function (evt) {
							if (evt?.target?.result) {
								resolve(evt.target.result);
							} else {
								reject("No VALUE");
							}
						};
						reader.readAsText(fileToParse);
					});
				};
				const fileInString = await parseFile(files[0]);
				parsed = JSON.parse(fileInString);
			}
		} catch (e) {
			debugger;
			console.log(e);
		}
	};
	const updater = (newParsed) => {
		parsed = newParsed;
	};
</script>

<main>
	<h1 class="center">JSON-EDITOR</h1>
	{#if parsed === null}
		<div class="center">
			<input
				type="file"
				bind:files
				on:change={() => {
					parsefiles();
				}}
			/><br />
			<button
				on:click={() => {
					parsed = [];
				}}
			>
				New Array
			</button>
			<button
				on:click={() => {
					parsed = {};
				}}
			>
				New Object
			</button>
		</div>
	{:else}
		<div class="cut">
			<div>
				<RenderStruct callback={updater} valueProps={parsed} />
			</div>
			<pre>{reactiveParsed}</pre>
		</div>
	{/if}
</main>

<style>
	.cut {
		display: flex;
	}
	.cut > * {
		flex: 0.5;
	}
	main {
		max-width: 240px;
		margin: 0 auto;
	}
	.center {
		margin: auto;
		text-align: center;
	}
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	pre {
		background-color: lightblue;
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
