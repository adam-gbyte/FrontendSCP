<script>
	import Navbar from '$lib/components/Navbar.svelte';
	import { onMount } from 'svelte';
</script>

<svelte:head>
	<title>Klasifikasi Kematangan Pisang Cavendish</title>
</svelte:head>

<div class="flex min-h-screen flex-col bg-yellow-50">
	<section class="flex flex-1 flex-col items-center justify-between px-12 py-10 md:flex-row">
		<div class="max-w-lg space-y-6">
			<h1 class="text-4xl leading-snug font-extrabold text-green-800 md:text-5xl">
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
				<a
					href="/jurnal"
					class="rounded-xl border border-green-600 px-6 py-3 font-semibold text-green-700 shadow-md transition hover:bg-green-50"
					>Jurnal</a
				>
			</div>
		</div>

		<img src="/logo.jpeg" alt="Pisang Cavendish" class="w-[350px] md:w-[420px]" />
	</section>
</div>
