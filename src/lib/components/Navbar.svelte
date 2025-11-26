<script>
    	import { onMount } from 'svelte';
	import { Moon, Sun, Menu, X } from 'lucide-svelte';
	// MOBILE MENU TOGGLER
	let isMenuOpen = false;
	function toggleMenu() {
		isMenuOpen = !isMenuOpen;
	}
</script>

<nav
	class={`fixed top-0 left-0 z-50 flex w-full items-center justify-between bg-gray-100 px-10 py-6 md:flex-row dark:bg-gray-900 dark:text-white`}
>
	<h1 class="text-xl font-bold text-gray-900 md:text-2xl dark:text-gray-200">Klasifikasi Pisang</h1>
    <!-- <img src="/logo.jpeg" alt="Logo Pisang" class="h-7 w-7" /> -->

	<div class={`hidden items-center space-x-6 md:flex`}>
		<ul class="flex items-center space-x-8 text-sm font-semibold">
			<li><a href="/" class="hover:text-[#ff48c4]">HOME</a></li>
			<li><a href="/klasifikasi" class="hover:text-[#ff48c4]">KLASIFIKASI</a></li>
			<li><a href="/riwayat" class="hover:text-[#ff48c4]">RIWAYAT</a></li>
			<li><a href="/tentang" class="hover:text-[#ff48c4]">TENTANG</a></li>
        </ul>
	</div>
</nav>

<!-- Mobile Menu Button -->
<button
	class="fixed top-4 right-4 z-70 cursor-pointer rounded-md border border-gray-300 bg-gray-100 p-2 text-gray-800 md:hidden dark:border-gray-700 dark:bg-gray-800 dark:text-gray-100"
	on:click={toggleMenu}
	aria-label="Toggle Menu"
>
	{#if isMenuOpen}
		<X />
	{:else}
		<Menu />
	{/if}
</button>

<aside
	class={`fixed top-0 right-0 z-60 h-full w-64 transform rounded-l-2xl bg-gray-300 p-4 shadow-lg transition-transform duration-500 ease-in-out md:hidden dark:bg-gray-700 dark:text-white ${
		isMenuOpen ? 'translate-x-0' : 'translate-x-full'
	}`}
>
	<h2 class="text-lg font-bold">Adam</h2>
	<ul class="flex w-full flex-col pt-8 pb-4 text-sm font-semibold">
		<li>
			<a
				href="/"
				class="flex rounded-t-xl bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4] dark:bg-gray-800 dark:hover:bg-gray-900"
				>HOME</a
			>
		</li>
		<li>
			<a
				href="/klasifikasi"
				class="flex bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4] dark:bg-gray-800 dark:hover:bg-gray-900"
				>KLASIFIKASI</a
			>
		</li>
		<li>
			<a
				href="/riwayat"
				class="flex bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4] dark:bg-gray-800 dark:hover:bg-gray-900"
				>RIWAYAT</a
			>
		</li>
		<li>
			<a
				href="/tentang"
				class="flex bg-gray-100 rounded-b-xl px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4] dark:bg-gray-800 dark:hover:bg-gray-900"
				>TENTANG</a
			>
		</li>
	</ul>
</aside>
