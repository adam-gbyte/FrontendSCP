<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import Navbar from '$lib/components/Navbar.svelte';
	import Footer from '$lib/components/Footer.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<title>Cavendish</title>
	<link rel="icon" href={favicon} />
</svelte:head>

<Navbar />

<main class="flex flex-col items-center justify-center">
	{@render children?.()}
</main>

<Footer />
