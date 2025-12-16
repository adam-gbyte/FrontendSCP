<script>
	let file = null;
	let previewUrl = '';
	let result = '';
	let loading = '';

	function handleFileChange(event) {
		file = event.target.files[0];
		if (file) {
			previewUrl = URL.createObjectURL(file);
		}
	}

	async function sendImage() {
		if (!file) {
			alert('Silakan pilih gambar terlebih dahulu.');
			return;
		}

		result = '';
		loading = 'Mengirim gambar dan memproses...';

		const formData = new FormData();
		formData.append('image', file);

		try {
			const response = await fetch('http://localhost:5000/api/predict', {
				method: 'POST',
				body: formData
			});

			const data = await response.json();
			loading = '';

			if (data.error) {
				result = 'Error: ' + data.error;
			} else {
				result = `Hasil Prediksi: ${data.prediction}`;
			}
		} catch (err) {
			loading = '';
			result = 'Terjadi kesalahan: ' + err;
		}
	}
</script>

<section
	id="klasifikasi"
	class="scroll-mt-25 bg-linear-to-br flex min-h-dvh w-screen items-center justify-center from-yellow-50 to-white p-6"
>
	<div class="w-full max-w-lg rounded-2xl bg-white p-6 shadow-xl">
		<!-- Header -->
		<div class="mb-6 text-center">
			<h2 class="text-2xl font-bold text-gray-800">🍌 Prediksi Gambar Pisang</h2>
			<p class="mt-1 text-sm text-gray-500">Upload gambar untuk melihat hasil prediksi</p>
		</div>

		<!-- Upload Area -->
		<label
			class="flex cursor-pointer flex-col items-center justify-center rounded-xl border-2 border-dashed border-yellow-400 bg-yellow-50 p-6 text-center transition hover:bg-yellow-100"
		>
			<input type="file" accept="image/*" on:change={handleFileChange} class="hidden" />
			<span class="text-lg font-medium text-yellow-700"> Klik untuk upload gambar </span>
			<span class="mt-1 text-sm text-gray-500"> PNG, JPG, JPEG </span>
		</label>

		<!-- Preview -->
		{#if previewUrl}
			<div class="mt-4">
				<img src={previewUrl} alt="Preview" class="mx-auto max-h-64 rounded-xl shadow-md" />
			</div>
		{/if}

		<!-- Button -->
		<button
			on:click={sendImage}
			class="mt-6 w-full rounded-xl bg-yellow-500 py-3 font-semibold text-white transition hover:bg-yellow-600 active:scale-[0.98]"
		>
			Prediksi Sekarang
		</button>

		<!-- Loading -->
		{#if loading}
			<p class="mt-4 animate-pulse text-center text-sm text-gray-500">⏳ Memproses gambar...</p>
		{/if}

		<!-- Result -->
		{#if result}
			<div class="mt-6 rounded-xl bg-green-50 p-4 text-center">
				<p class="text-lg font-bold text-green-700">
					{@html result}
				</p>
			</div>
		{/if}
	</div>
</section>
