<script>
	import { onMount } from 'svelte';
	let searchQuery = '';
	let riwayat = [
		{
			id: 1,
			nama: 'pisang1.jpg',
			hasil: 'Matang',
			confidence: 93.2,
			waktu: '0.85 s',
			tanggal: '26 Okt 2025, 12:10'
		},
		{
			id: 2,
			nama: 'pisang2.jpg',
			hasil: 'Mentah',
			confidence: 88.7,
			waktu: '0.79 s',
			tanggal: '26 Okt 2025, 12:12'
		},
		{
			id: 3,
			nama: 'pisang3.jpg',
			hasil: 'Setengah Matang',
			confidence: 90.1,
			waktu: '0.82 s',
			tanggal: '26 Okt 2025, 12:14'
		}
	];

	let filtered = riwayat;

	function cari() {
		filtered = riwayat.filter(
			(item) =>
				item.hasil.toLowerCase().includes(searchQuery.toLowerCase()) ||
				item.nama.toLowerCase().includes(searchQuery.toLowerCase())
		);
	}

	function hapusSemua() {
		if (confirm('Apakah Anda yakin ingin menghapus semua riwayat?')) {
			filtered = [];
			riwayat = [];
		}
	}

	function muatUlang() {
		filtered = [...riwayat];
	}
</script>

<svelte:head>
	<title>Riwayat Klasifikasi Pisang Cavendish</title>
</svelte:head>

<div class="flex min-h-screen flex-col bg-yellow-50">
	<!-- Riwayat Section -->
	<section class="flex-1 px-12 py-10">
		<h1 class="mb-6 text-3xl font-bold text-green-900">Riwayat Klasifikasi</h1>

		<div class="mb-6 flex flex-col justify-between gap-4 md:flex-row md:items-center">
			<input
				type="text"
				placeholder="🔍 Cari data..."
				bind:value={searchQuery}
				on:input={cari}
				class="w-full rounded-xl border border-green-300 p-3 focus:outline-none focus:ring-2 focus:ring-green-400 md:w-1/2"
			/>

			<select
				class="rounded-xl border border-green-300 bg-white p-3 focus:outline-none focus:ring-2 focus:ring-green-400"
			>
				<option>Urutkan berdasarkan tanggal</option>
			</select>
		</div>

		<!-- Table -->
		<div class="overflow-x-auto">
			<table class="w-full border-collapse overflow-hidden rounded-xl">
				<thead>
					<tr class="bg-green-100 text-green-900">
						<th class="px-4 py-3 text-left">No</th>
						<th class="px-4 py-3 text-left">Nama File</th>
						<th class="px-4 py-3 text-left">Hasil Klasifikasi</th>
						<th class="px-4 py-3 text-left">Confidence (%)</th>
						<th class="px-4 py-3 text-left">Waktu Proses</th>
						<th class="px-4 py-3 text-left">Tanggal & Waktu</th>
					</tr>
				</thead>
				<tbody>
					{#if filtered.length > 0}
						{#each filtered as item, i}
							<tr class="border-b hover:bg-green-50">
								<td class="px-4 py-3">{i + 1}</td>
								<td class="px-4 py-3">{item.nama}</td>
								<td class="px-4 py-3">{item.hasil}</td>
								<td class="px-4 py-3">{item.confidence}</td>
								<td class="px-4 py-3">{item.waktu}</td>
								<td class="px-4 py-3">{item.tanggal}</td>
							</tr>
						{/each}
					{:else}
						<tr>
							<td colspan="6" class="py-6 text-center text-gray-500"> Tidak ada data riwayat </td>
						</tr>
					{/if}
				</tbody>
			</table>
		</div>

		<!-- Action Buttons -->
		<div class="mt-6 flex gap-4">
			<button
				on:click={hapusSemua}
				class="rounded-xl bg-red-600 px-6 py-3 font-semibold text-white shadow-md transition hover:bg-red-700"
			>
				🗑️ Hapus Semua Riwayat
			</button>

			<button
				on:click={muatUlang}
				class="rounded-xl bg-green-600 px-6 py-3 font-semibold text-white shadow-md transition hover:bg-green-700"
			>
				🔄 Muat Ulang Data
			</button>
		</div>
	</section>
</div>
