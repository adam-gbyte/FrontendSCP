<script>
	import { onMount, onDestroy } from 'svelte';
	let videoEl;
	let canvasEl;
	let stream = null;
	let devices = [];
	let selectedDeviceId = null;
	let photoDataUrl = null;
	let taking = false;

	// mulai kamera dengan deviceId (atau default)
	async function startCamera(deviceId = null, facingMode = null) {
		stopCamera();
		try {
			const constraints = deviceId
				? { video: { deviceId: { exact: deviceId } } }
				: facingMode
					? { video: { facingMode } }
					: { video: { width: { ideal: 1280 }, height: { ideal: 720 } } };

			stream = await navigator.mediaDevices.getUserMedia(constraints);
			videoEl.srcObject = stream;
			await videoEl.play();
		} catch (err) {
			console.error('Gagal akses kamera:', err);
			alert(
				'Tidak bisa mengakses kamera. Pastikan izin diberikan dan jalankan lewat HTTPS atau localhost.'
			);
		}
	}

	function stopCamera() {
		if (stream) {
			stream.getTracks().forEach((t) => t.stop());
			stream = null;
		}
		if (videoEl) videoEl.pause();
	}

	// ambil foto ke canvas, simpan dataURL
	function takePhoto() {
		if (!videoEl) return;
		taking = true;
		const w = videoEl.videoWidth;
		const h = videoEl.videoHeight;
		canvasEl.width = w;
		canvasEl.height = h;
		const ctx = canvasEl.getContext('2d');
		ctx.drawImage(videoEl, 0, 0, w, h);
		photoDataUrl = canvasEl.toDataURL('image/jpeg', 0.92);
		taking = false;
	}

	// download foto
	function downloadPhoto() {
		if (!photoDataUrl) return;
		const a = document.createElement('a');
		a.href = photoDataUrl;
		a.download = `photo_${Date.now()}.jpg`;
		a.click();
	}

	// upload ke server (/api/upload)
	async function uploadPhoto() {
		if (!photoDataUrl) return;
		// ubah dataURL jadi blob
		const blob = await (await fetch(photoDataUrl)).blob();
		const fd = new FormData();
		fd.append('photo', blob, `photo_${Date.now()}.jpg`);
		const res = await fetch('/api/upload', {
			method: 'POST',
			body: fd
		});
		if (!res.ok) {
			const txt = await res.text();
			alert('Upload gagal: ' + txt);
		} else {
			const j = await res.json();
			alert('Upload sukses: ' + (j.message ?? 'OK'));
		}
	}

	// ambil daftar device kamera untuk pilih kamera belakang/depannya
	async function enumerateDevices() {
		const all = await navigator.mediaDevices.enumerateDevices();
		devices = all.filter((d) => d.kind === 'videoinput');
		// default: pilih device pertama
		if (!selectedDeviceId && devices.length) {
			selectedDeviceId = devices[0].deviceId;
		}
	}

	// saat user ganti pilihan kamera
	async function onChangeDevice() {
		if (selectedDeviceId) {
			await startCamera(selectedDeviceId);
		}
	}

	onMount(async () => {
		if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
			alert('Browser Anda tidak mendukung kamera (getUserMedia). Gunakan browser modern.');
			return;
		}
		await enumerateDevices();
		// coba gunakan kamera belakang bila tersedia
		const back = devices.find((d) => /back|rear|environment/i.test(d.label));
		if (back) {
			selectedDeviceId = back.deviceId;
			await startCamera(selectedDeviceId);
		} else {
			await startCamera(); // default
		}
	});

	onDestroy(() => stopCamera());
</script>

<svelte:head>
	<title>Analisis Kematangan Pisang Cavendish</title>
</svelte:head>

<h1>DETEKSI KEMATANGAN PISANG CAVENDISH</h1>

<div class="camera">
	<div>
		<video bind:this={videoEl} autoplay playsinline muted></video>
		<canvas bind:this={canvasEl}></canvas>
	</div>

	<div class="controls">
		<label>
			Pilih Kamera:
			<select bind:value={selectedDeviceId} on:change={onChangeDevice}>
				{#if devices.length === 0}
					<option disabled>Memuat perangkat...</option>
				{/if}
				{#each devices as d}
					<option value={d.deviceId}>{d.label || `Camera ${d.deviceId}`}</option>
				{/each}
			</select>
		</label>

		<div>
			<button on:click={() => takePhoto()} disabled={taking}>📸 Ambil Foto</button>
			<button on:click={() => startCamera(null, 'environment')}>🔄 Back (facingMode)</button>
			<button on:click={() => startCamera(null, 'user')}>🔄 Front (facingMode)</button>
			<button on:click={stopCamera}>⛔ Stop Kamera</button>
		</div>

		{#if photoDataUrl}
			<div>
				<p>Preview:</p>
				<img class="preview" src={photoDataUrl} alt="Preview foto" />
				<div style="margin-top:.5rem;">
					<button on:click={downloadPhoto}>⬇️ Download</button>
					<button on:click={uploadPhoto}>⬆️ Upload ke server</button>
				</div>
			</div>
		{/if}
	</div>
</div>

<style>
	.camera {
		display: flex;
		gap: 1rem;
		align-items: flex-start;
		flex-wrap: wrap;
	}
	video {
		max-width: 400px;
		border-radius: 8px;
		background: #000;
	}
	canvas {
		display: none;
	} /* canvas internal */
	.controls {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}
	.preview {
		max-width: 400px;
		border-radius: 8px;
	}
	select,
	button {
		padding: 0.4rem 0.6rem;
		border-radius: 6px;
		border: 1px solid #ddd;
		background: #fff;
		cursor: pointer;
	}
</style>
