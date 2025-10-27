<script>
	import { onMount } from 'svelte';
</script>

<svelte:head>
	<title>Tentang Sistem Klasifikasi Pisang Cavendish</title>
</svelte:head>

<div class="flex min-h-screen flex-col bg-yellow-50">
	<!-- Navbar -->
	<nav class="flex items-center justify-between bg-white px-8 py-4 shadow-sm">
		<div class="flex items-center gap-2 text-xl font-semibold text-green-700">
			<img src="/logo.jpeg" alt="Logo Pisang" class="h-7 w-7" />
			<span>Klasifikasi Pisang</span>
		</div>

		<ul class="flex gap-8 font-medium text-gray-700">
			<li><a href="/" class="hover:text-green-700">Home</a></li>
			<li>
				<a href="/klasifikasi" class="hover:text-green-700"> Klasifikasi </a>
			</li>
			<li><a href="/riwayat" class="hover:text-green-700">Riwayat</a></li>
			<li>
				<a href="/tentang" class="border-b-2 border-green-700 pb-1 font-semibold text-green-700">
					Tentang
				</a>
			</li>
		</ul>
	</nav>

	<!-- Content Section -->
	<section class="flex flex-1 flex-col items-center justify-between px-12 py-10 md:flex-row">
		<!-- Text Content -->
		<div class="max-w-xl space-y-6">
			<h2 class="text-2xl font-bold text-green-800">Tentang Sistem</h2>
			<h1 class="text-4xl font-extrabold leading-snug text-green-900 md:text-5xl">
				Klasifikasi Kematangan <br />
				<span class="text-yellow-600">Buah Pisang</span>
			</h1>

			<p class="leading-relaxed text-gray-700">
				Sistem ini bertujuan membantu petani dalam menentukan tingkat kematangan buah pisang
				Cavendish dengan cepat dan akurat. Model <span class="font-semibold">MobileNetV2</span>
				digunakan untuk mengambil fitur citra buah, sedangkan algoritma
				<span class="font-semibold">SVM</span>
				digunakan untuk mengklasifikasikan buah ke dalam tiga kelas: <strong>mentah</strong>,
				<strong>setengah matang</strong>, dan <strong>matang</strong>.
			</p>

			<div class="rounded-xl border border-yellow-200 bg-white p-4 shadow-md">
				<ul class="space-y-2 text-gray-800">
					<li>
						<span class="font-semibold text-green-700">Frontend:</span> HTML, CSS, JavaScript (SvelteKit)
					</li>
					<li>
						<span class="font-semibold text-green-700">Backend:</span> Python / Node.js
					</li>
					<li>
						<span class="font-semibold text-green-700">Database:</span> MongoDB
					</li>
					<li>
						<span class="font-semibold text-green-700">Model:</span> MobileNetV2 + SVM
					</li>
				</ul>
			</div>
		</div>

		<!-- Image -->
		<div class="mt-10 md:mt-0">
			<img src="/logo.jpeg" alt="Pisang Cavendish" class="w-[350px] md:w-[420px]" />
			<div class="mt-2 flex justify-between text-sm text-gray-700">
				<span>Mentah</span>
				<span>Matang</span>
			</div>
		</div>
	</section>

	<!-- Footer -->
	<footer class="bg-yellow-100 py-4 text-center text-sm text-gray-600">
		© 2025 Sistem Klasifikasi Kematangan Pisang Cavendish – Universitas Nusa Putra
	</footer>
</div>
