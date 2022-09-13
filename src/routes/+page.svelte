<script lang="ts">
	import { onMount } from 'svelte';
	import { Book, Rendition } from 'epubjs';
	let reader;

	onMount(() => {
		let meta, title, author, cover, progress, info, sliderValue, isReady;
		const book = new Book('/venganza.epub', {});
		const rendition = new Rendition(book, {
			width: '100%',
			height: '100%'
		});

		rendition.on('rendered', (e, iframe) => {
			console.log('rendered', e, iframe);
			iframe.iframe.contentWindow.focus();
			// clickListener(iframe.document, rendition, flipPage);
			// selectListener(iframe.document, rendition, toggleBuble);
			// swipListener(iframe.document, flipPage);
			// wheelListener(iframe.document, flipPage);
			// keyListener(iframe.document, flipPage);
		});

		rendition.on('relocated', (location) => {
			info.lastCfi = location.start.cfi;
			// $db.set(info.id, info);
			// history.push(location.start.cfi);

			progress = book?.locations.percentageFromCfi(location.start.cfi);
			sliderValue = Math.floor(progress * 10000) / 100;
		});

		// Apply style https://github.dev/Janglee123/eplee
		// rendition.hooks.content.register(applyStyle);

		book.ready
			.then(() => {
				console.log('book ready', reader, book, rendition);
				// meta = book.package.metadata;
				// title = meta.title;
				// book?.locations?.load(info?.locations);
			})
			.then(() => {
				rendition.attachTo(reader);
				rendition.display(info?.lastCfi || 1);
				// rendition.themes.registerRules('dark', dark);
				// rendition.themes.registerRules('tan', tan);
				rendition.ready = true;
				// theme = $store.getters.theme;
				// applytheme(theme);
			})
			.then(() => {
				info.highlights.forEach((cfiRange) => {
					rendition.annotations.highlight(cfiRange);
				});
			})
			.then(() => {
				isReady = true;
				console.log(book);
				console.log(rendition);
			});
	});
</script>

<svelte:head>
	<title>Top Svelte</title>
</svelte:head>

<div>
	<div bind:this={reader} id="reader" class=" h-[70vh] bg-red-100 text-black" />
</div>

<style>
	#title {
		width: 900px;
		min-height: 18px;
		margin: 10px auto;
		text-align: center;
		font-size: 16px;
		color: #e2e2e2;
		font-weight: 400;
	}

	#title:hover {
		color: #777;
	}

	#viewer.spreads {
		width: 900px;
		height: 600px;
		box-shadow: 0 0 4px #ccc;
		border-radius: 5px;
		padding: 0;
		position: relative;
		margin: 10px auto;
		background: white url('ajax-loader.gif') center center no-repeat;
		top: calc(50vh - 400px);
	}

	#viewer.spreads .epub-view > iframe {
		background: white;
	}

	#viewer.scrolled {
		overflow: hidden;
		width: 800px;
		margin: 0 auto;
		position: relative;
		background: url('ajax-loader.gif') center center no-repeat;
		box-shadow: 0 0 4px #ccc;
		padding: 20px;
		background: white;
	}

	#viewer.scrolled .epub-view > iframe {
		background: white;
	}

	#prev {
		left: 0;
	}

	#next {
		right: 0;
	}

	#toc {
		display: block;
		margin: 10px auto;
	}

	@media (min-width: 1000px) {
		#viewer.spreads:after {
			position: absolute;
			width: 1px;
			border-right: 1px #000 solid;
			height: 90%;
			z-index: 1;
			left: 50%;
			margin-left: -1px;
			top: 5%;
			opacity: 0.15;
			box-shadow: -2px 0 15px rgba(0, 0, 0, 1);
			content: '';
		}

		#viewer.spreads.single:after {
			display: none;
		}

		#prev {
			left: 40px;
		}

		#next {
			right: 40px;
		}
	}

	.arrow {
		position: fixed;
		top: 50%;
		margin-top: -32px;
		font-size: 64px;
		color: #e2e2e2;
		font-family: arial, sans-serif;
		font-weight: bold;
		cursor: pointer;
		-webkit-user-select: none;
		-moz-user-select: none;
		user-select: none;
		text-decoration: none;
	}

	.navlink {
		margin: 14px;
		display: block;
		text-align: center;
		text-decoration: none;
		color: #ccc;
	}

	.arrow:hover,
	.navlink:hover {
		color: #777;
	}

	.arrow:active,
	.navlink:hover {
		color: #000;
	}

	#book-wrapper {
		width: 480px;
		height: 640px;
		overflow: hidden;
		border: 1px solid #ccc;
		margin: 28px auto;
		background: #fff;
		border-radius: 0 5px 5px 0;
		position: absolute;
	}

	#book-viewer {
		width: 480px;
		height: 660px;
		margin: -30px auto;
		-moz-box-shadow: inset 10px 0 20px rgba(0, 0, 0, 0.1);
		-webkit-box-shadow: inset 10px 0 20px rgba(0, 0, 0, 0.1);
		box-shadow: inset 10px 0 20px rgba(0, 0, 0, 0.1);
	}

	#book-viewer iframe {
		padding: 40px 40px;
	}

	#controls {
		position: absolute;
		bottom: 16px;
		left: 50%;
		width: 400px;
		margin-left: -200px;
		text-align: center;
		display: none;
	}

	#controls > input[type='range'] {
		width: 400px;
	}

	#navigation {
		width: 400px;
		height: 100vh;
		position: absolute;
		overflow: auto;
		top: 0;
		left: 0;
		background: #777;
		-webkit-transition: -webkit-transform 0.25s ease-out;
		-moz-transition: -moz-transform 0.25s ease-out;
		-ms-transition: -moz-transform 0.25s ease-out;
		transition: transform 0.25s ease-out;
	}

	#navigation.fixed {
		position: fixed;
	}

	#navigation h1 {
		width: 200px;
		font-size: 16px;
		font-weight: normal;
		color: #fff;
		margin-bottom: 10px;
	}

	#navigation h2 {
		font-size: 14px;
		font-weight: normal;
		color: #b0b0b0;
		margin-bottom: 20px;
	}

	#navigation ul {
		padding-left: 36px;
		margin-left: 0;
		margin-top: 12px;
		margin-bottom: 12px;
		width: 340px;
	}

	#navigation ul li {
		list-style: decimal;
		margin-bottom: 10px;
		color: #cccddd;
		font-size: 12px;
		padding-left: 0;
		margin-left: 0;
	}

	#navigation ul li a {
		color: #ccc;
		text-decoration: none;
	}

	#navigation ul li a:hover {
		color: #fff;
		text-decoration: underline;
	}

	#navigation ul li a.active {
		color: #fff;
	}

	#navigation #cover {
		display: block;
		margin: 24px auto;
	}

	#navigation #closer {
		position: absolute;
		top: 0;
		right: 0;
		padding: 12px;
		color: #cccddd;
		width: 24px;
	}

	#navigation.closed {
		-webkit-transform: translate(-400px, 0);
		-moz-transform: translate(-400px, 0);
		-ms-transform: translate(-400px, 0);
		transform: translate(-400px, 0);
	}

	svg {
		display: block;
	}

	.close-x {
		stroke: #cccddd;
		fill: transparent;
		stroke-linecap: round;
		stroke-width: 5;
	}

	.close-x:hover {
		stroke: #fff;
	}

	#opener {
		position: absolute;
		top: 0;
		left: 0;
		padding: 10px;
		stroke: #e2e2e2;
		fill: #e2e2e2;
	}

	#opener:hover {
		stroke: #777;
		fill: #777;
	}
</style>
