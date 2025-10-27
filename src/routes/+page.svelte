<script>
	import { onMount } from 'svelte';
</script>

<svelte:head>
	<title>Klasifikasi Kematangan Pisang Cavendish</title>
</svelte:head>

<div class="flex min-h-screen flex-col bg-yellow-50">
	<!-- Navbar -->
	<nav class="flex items-center justify-between bg-white px-8 py-4 shadow-sm">
		<div class="flex items-center gap-2 text-xl font-semibold text-green-700">
			<img src="/logo.jpeg" alt="Logo Pisang" class="h-7 w-7" />
			<span>Klasifikasi Pisang</span>
		</div>

		<ul class="flex gap-8 font-medium text-gray-700">
			<li>
				<a href="/" class="border-b-2 border-green-700 pb-1 font-semibold text-green-700"> Home </a>
			</li>
			<li><a href="/klasifikasi" class="hover:text-green-700">Klasifikasi</a></li>
			<li><a href="/riwayat" class="hover:text-green-700">Riwayat</a></li>
			<li><a href="/tentang" class="hover:text-green-700">Tentang</a></li>
		</ul>
	</nav>

	<!-- Hero Section -->
	<section class="flex flex-1 flex-col items-center justify-between px-12 py-10 md:flex-row">
		<div class="max-w-lg space-y-6">
			<h1 class="text-4xl font-extrabold leading-snug text-green-800 md:text-5xl">
				Klasifikasi Kematangan <br />
				<span class="text-yellow-600">Buah Pisang Cavendish</span>
			</h1>
			<p class="leading-relaxed text-gray-700">
				Sistem ini mendeteksi tingkat kematangan buah Cavendish menggunakan model
				<span class="font-semibold">MobileNetV2</span> dan <span class="font-semibold">SVM</span> secara
				otomatis berdasarkan citra buah.
			</p>

			<div class="flex gap-4">
				<a
					href="/klasifikasi"
					class="rounded-xl bg-green-600 px-6 py-3 font-semibold text-white shadow-md transition hover:bg-green-700"
					>🍌 Mulai Klasifikasi</a
				>
				<a
					href="/riwayat"
					class="rounded-xl border border-green-600 px-6 py-3 font-semibold text-green-700 shadow-md transition hover:bg-green-50"
					>🔍 Lihat Riwayat</a
				>
			</div>
		</div>

		<img src="/logo.jpeg" alt="Pisang Cavendish" class="w-[350px] md:w-[420px]" />
	</section>

	<!-- Footer -->
	<footer class="bg-yellow-100 py-4 text-center text-sm text-gray-600">
		© 2025 Sistem Klasifikasi Kematangan Pisang Cavendish – Universitas Nusa Putra
	</footer>
</div>
