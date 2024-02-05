<script lang="ts">
	import QRCode from 'qrcode';
	import { onMount } from 'svelte';

	const errorCorrectionLevels = ['L', 'M', 'Q', 'H'];

	let content = 'Any text works here 🐝';
	let img: HTMLImageElement;
	let foregroundColor = '#000000';
	let backgroundColor = '#FFFFFF';
	let selectedErrorCorrectionLevel = errorCorrectionLevels[3];

	let opts = {
		errorCorrectionLevel: errorCorrectionLevels,
		type: 'image/png',
		margin: 1,
		width: '384',
		color: {
			dark: foregroundColor,
			light: backgroundColor
		}
	};

	function generateQRCode(e: Event) {
		// @ts-ignore
		QRCode.toDataURL(content, opts, function (err, url) {
			if (err) throw err;

			img.src = url;
		});
	}

	function changeErrorCorrectionLevel(e: any) {
		opts.errorCorrectionLevel = e.target.value;

		opts = opts;

		generateQRCode(e);
	}

	function changeColor(e: any) {
		if (e.target.name === 'foregroundColor') {
			opts.color.dark = e.target.value;
		} else {
			opts.color.light = e.target.value;
		}

		opts = opts;

		generateQRCode(e);
	}

	onMount(() => {
		// @ts-ignore
		QRCode.toDataURL(content, opts, function (err, url) {
			if (err) throw err;

			img.src = url;
		});
	});
</script>

<h1
	class="text-4xl font-mono font-bold text-center underline underline-offset-4 decoration-wavy decoration-purple-600"
>
	QR Generator
</h1>

<section class="flex flex-col font-mono gap-2">
	<div class="flex flex-col gap-1">
		<label for="content">Content</label>
		<textarea
			name="content"
			bind:value={content}
			on:input={generateQRCode}
			class="text-black px-2 py-1 font-sans"
		/>
	</div>

	<div class="text-black flex flex-col gap-1">
		<label for="errorCorrectionLevel" class="text-white">Error Correction Level</label>
		<select
			name="errorCorrectionLevel"
			bind:value={selectedErrorCorrectionLevel}
			on:change={changeErrorCorrectionLevel}
			class="self-start"
		>
			{#each errorCorrectionLevels as errorLevel}
				<option value={errorLevel}>{errorLevel}</option>
			{/each}
		</select>
	</div>

	<div class="flex flex-col gap-1">
		<label for="foregroundColor">Foreground Color</label>
		<input
			name="foregroundColor"
			type="color"
			bind:value={foregroundColor}
			on:change={changeColor}
		/>
	</div>

	<div class="flex flex-col gap-1">
		<label for="backgroundColor">Background Color</label>
		<input
			name="backgroundColor"
			type="color"
			bind:value={backgroundColor}
			on:change={changeColor}
		/>
	</div>

	<div class="self-center">
		<img alt="generated" bind:this={img} class="block w-96" />
	</div>
</section>

<style>
	/* input[type='color'] {
		border: none;
	} */
	input[type='color']::-webkit-color-swatch-wrapper {
		padding: 0;
	}
	input[type='color']::-webkit-color-swatch {
		border: none;
	}
</style>
