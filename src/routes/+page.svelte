<script>
	import Navbar from '$lib/components/Navbar.svelte';
	import { onMount } from 'svelte';
</script>

<section id="home" class="scroll-mt-25 mt-20 flex min-h-dvh p-6">
	<div class="flex grid-cols-2 flex-col items-center sm:flex-row">
		<div class="flex flex-col">
			<h1 class="text-4xl font-extrabold leading-snug text-green-800 md:text-5xl">
				Klasifikasi Kematangan <br />
				<span class="text-yellow-600">Buah Pisang Cavendish</span>
			</h1>
			<p class="leading-relaxed text-gray-700">
				Sistem ini mendeteksi tingkat kematangan buah Cavendish menggunakan model
				<span class="font-semibold">MobileNetV2</span> dan <span class="font-semibold">SVM</span> secara
				otomatis berdasarkan citra buah.
			</p>

			<div class="mb-3 flex flex-col gap-3 pt-3 md:flex-row md:p-3">
				<a
					href="/klasifikasi"
					class="rounded-xl bg-green-600 px-6 py-3 font-semibold text-white shadow-md transition hover:bg-green-700"
					>Klasifikasi</a
				>

				<a
					href="/riwayat"
					class="rounded-xl border border-green-600 px-6 py-3 font-semibold text-green-700 shadow-md transition hover:bg-green-50"
					>Riwayat</a
				>

				<a
					href="/jurnal"
					class="rounded-xl border border-green-600 px-6 py-3 font-semibold text-green-700 shadow-md transition hover:bg-green-50"
					>Jurnal</a
				>
			</div>
		</div>
		<img src="/logo.jpeg" alt="Pisang Cavendish" class="h-lg w-lg pt-3" />
	</div>
</section>
