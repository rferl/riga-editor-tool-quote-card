<!-- This is the Settings component written by the tool developer. -->

<script>import { onMount } from "svelte";
import ColorPicker from "./controls/ColorPicker.svelte";
import DropDown from "./controls/Dropdown.svelte";
import config from "./riga-tool.config.js";
import { decodeHtml } from "../utils/index.js";
import { updateOutput } from "../output/index.js";
export let settings;
export let id;
export let output;
const defaultSettings = config.settings;
let isLoaded = false;
onMount(async () => {
  $settings = { ...defaultSettings, ...$settings };
  isLoaded = true;
});
const quoteSymbolDropDown = [
  { value: "&#10078;", label: decodeHtml("&#10078;") },
  { value: "&#10077;", label: decodeHtml("&#10077;") },
  { value: "&#10076;", label: decodeHtml("&#10076;") },
  { value: "&#10075;", label: decodeHtml("&#10075;") }
];
$:
  if ($settings)
    updateOutput(id, settings, output);
</script>

{#if isLoaded}
	<form class="rt-form">
		<fieldset class="rt-fieldset">
			<legend class="rt-legend">Card Settings</legend>
			<div class="rt-setting w-full">
				<label for="quote-text" class="rt-label">Quote text</label>
				<textarea
					name="quote-text"
					id="quote-text"
					cols="10"
					rows="3"
					class="rt-input font-skolar max-w-none text-gray-500"
					bind:value={$settings.quote_text}
				/>
			</div>
			<div class="rt-setting">
				<DropDown
					label="Quote symbol"
					list={quoteSymbolDropDown}
					{settings}
					setting={'quote_symbol'}
				/>
			</div>
			<div class="rt-setting">
				<label for="quote-text-size" class="rt-label">Text size</label>
				<input
					type="number"
					name="quote-text-size"
					id="quote-text-size"
					class="rt-input"
					bind:value={$settings.quote_text_size}
				/>
			</div>
			<div class="rt-setting">
				<ColorPicker label="Text color" {settings} setting="quote_text_color" />
			</div>
			<div class="rt-setting">
				<ColorPicker label="Background color" {settings} setting="quote_background_color" />
			</div>
			<div class="rt-setting">
				<ColorPicker label="Quote symbol color" {settings} setting="quote_symbol_color" />
			</div>
		</fieldset>
		<fieldset class="rt-fieldset">
			<legend class="rt-legend">Output Settings</legend>
			<div class="rt-setting w-full">
				<label for="iframe-title" class="rt-label">Iframe title</label>
				<input
					name="iframe-title"
					id="iframe-title"
					class="rt-input font-skolar max-w-none p-2 text-gray-500"
					bind:value={$settings.iframe_title}
				/>
			</div>
		</fieldset>
	</form>
{/if}
