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
	class={`fixed left-0 top-0 z-50 flex w-full items-center justify-between bg-gray-100 px-10 py-6 md:flex-row`}
>
	<h1 class="text-xl font-bold text-gray-900 md:text-2xl">Klasifikasi Pisang</h1>
	<!-- <img src="/logo.jpeg" alt="Logo Pisang" class="h-7 w-7" /> -->

	<div class={`hidden items-center space-x-6 md:flex`}>
		<ul class="flex items-center space-x-8 text-sm font-semibold">
			<li><a href="/" class="hover:text-[#ff48c4]">HOME</a></li>
			<li><a href="/klasifikasi" class="hover:text-[#ff48c4]">KLASIFIKASI</a></li>
			<li><a href="/riwayat" class="hover:text-[#ff48c4]">RIWAYAT</a></li>
			<li><a href="/tentang" class="hover:text-[#ff48c4]">TENTANG</a></li>
			<li><a href="/jurnal" class="hover:text-[#ff48c4]">JURNAL</a></li>
		</ul>
	</div>
</nav>

<!-- Mobile Menu Button -->
<button
	class="z-70 fixed right-4 top-4 cursor-pointer rounded-md border border-gray-300 bg-gray-100 p-2 text-gray-800 md:hidden"
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
	class={`z-60 fixed right-0 top-0 h-full w-64 transform rounded-l-2xl bg-gray-300 p-4 shadow-lg transition-transform duration-500 ease-in-out md:hidden ${
		isMenuOpen ? 'translate-x-0' : 'translate-x-full'
	}`}
>
	<h2 class="text-lg font-bold">Adam</h2>
	<ul class="flex w-full flex-col pb-4 pt-8 text-sm font-semibold">
		<li>
			<a
				href="/"
				class="flex rounded-t-xl bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4]"
				>HOME</a
			>
		</li>
		<li>
			<a
				href="/klasifikasi"
				class="flex bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4]">KLASIFIKASI</a
			>
		</li>
		<li>
			<a href="/riwayat" class="flex bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4]"
				>RIWAYAT</a
			>
		</li>
		<li>
			<a
				href="/tentang"
				class="flex rounded-b-xl bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4]"
				>TENTANG</a
			>
		</li>
		<li>
			<a
				href="/jurnal"
				class="flex rounded-b-xl bg-gray-100 px-4 py-2 hover:bg-gray-200 hover:text-[#ff48c4]"
				>JURNAL</a
			>
		</li>
	</ul>
</aside>
