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

<div class="container">
	<h2>Upload Gambar Pisang</h2>

	<input type="file" accept="image/*" on:change={handleFileChange} />

	<button on:click={sendImage}>Prediksi</button>

	{#if loading}
		<p class="loading">{loading}</p>
	{/if}

	{#if previewUrl}
		<img class="preview" src={previewUrl} alt="preview" />
	{/if}

	{#if result}
		<div class="result">{@html result}</div>
	{/if}
</div>

<style>
	.container {
		max-width: 400px;
		margin: auto;
	}
	input[type='file'] {
		margin-bottom: 15px;
	}
	.preview {
		width: 100%;
		margin-top: 20px;
		border-radius: 8px;
	}
	.result {
		margin-top: 20px;
		font-size: 20px;
		font-weight: bold;
		color: #333;
	}
	button {
		padding: 10px 20px;
		cursor: pointer;
		font-size: 16px;
	}
	.loading {
		margin-top: 10px;
		color: #555;
		font-style: italic;
	}
</style>
