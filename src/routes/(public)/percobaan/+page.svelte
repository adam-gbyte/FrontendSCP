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

<div class="mx-auto max-w-[400px] space-y-4 p-4">
	<h2 class="text-xl font-semibold">Upload Gambar Pisang</h2>

	<input
		type="file"
		accept="image/*"
		on:change={handleFileChange}
		class="block w-full cursor-pointer rounded-lg border border-gray-300 p-2"
	/>

	<button
		on:click={sendImage}
		class="rounded-lg bg-blue-600 px-4 py-2 text-white transition hover:bg-blue-700"
	>
		Prediksi
	</button>

	{#if loading}
		<p class="italic text-gray-600">{loading}</p>
	{/if}

	{#if previewUrl}
		<img class="mt-2 w-full rounded-xl shadow" src={previewUrl} alt="preview" />
	{/if}

	{#if result}
		<div class="mt-4 text-xl font-bold text-gray-800">
			{@html result}
		</div>
	{/if}
</div>
