<script lang="ts">
	import NativeFileApi from '$lib/NativeFileApi.svelte';
	import Carousel from '$lib/Carousel.svelte';
</script>

<svelte:head>
	<title>Top Svelte</title>
</svelte:head>

<div>
	<div class="text-3xl mt-20">Index page</div>
	<div class="prose">
		<ul>
			<li>SvelteKit</li>
			<li>TailwindCSS (with official plugins) + DaisyUI + FlowBir</li>
			<li>Theme Switcher</li>
			<li>Markdown Routes with mdsvex</li>
			<li>Static Blog with markdown files</li>
			<li>Supabase with Auth</li>
			<li>Electron</li>
			<li>Native File System API</li>
			<li />
		</ul>

		Custom Components:
		<ul>
			<li>Simple image carrousel</li>
		</ul>
	</div>

	<div class="my-5" />


  <h2 class=text-2xl>Native File API</h2>
	<NativeFileApi />



  <h2 class=text-2xl>Components</h2>
	<h3 class="text-xl mt-3">Image Carousel</h3>
	<Carousel />
</div>
