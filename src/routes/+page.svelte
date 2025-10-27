<script>
	import { onMount, onDestroy } from 'svelte';
	let videoEl;
	let canvasEl;
	let stream = null;
	let devices = [];
	let selectedDeviceId = null;
	let photoDataUrl = null;
	let taking = false;

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

	function downloadPhoto() {
		if (!photoDataUrl) return;
		const a = document.createElement('a');
		a.href = photoDataUrl;
		a.download = `photo_${Date.now()}.jpg`;
		a.click();
	}

	async function uploadPhoto() {
		if (!photoDataUrl) return;
		const blob = await (await fetch(photoDataUrl)).blob();
		const fd = new FormData();
		fd.append('photo', blob, `photo_${Date.now()}.jpg`);
		const res = await fetch('/api/upload', { method: 'POST', body: fd });
		if (!res.ok) {
			const txt = await res.text();
			alert('Upload gagal: ' + txt);
		} else {
			const j = await res.json();
			alert('Upload sukses: ' + (j.message ?? 'OK'));
		}
	}

	async function enumerateDevices() {
		const all = await navigator.mediaDevices.enumerateDevices();
		devices = all.filter((d) => d.kind === 'videoinput');
		if (!selectedDeviceId && devices.length) {
			selectedDeviceId = devices[0].deviceId;
		}
	}

	async function onChangeDevice() {
		if (selectedDeviceId) {
			await startCamera(selectedDeviceId);
		}
	}

	onMount(async () => {
		if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
			alert('Browser Anda tidak mendukung kamera. Gunakan browser modern.');
			return;
		}
		await enumerateDevices();
		const back = devices.find((d) => /back|rear|environment/i.test(d.label));
		if (back) {
			selectedDeviceId = back.deviceId;
			await startCamera(selectedDeviceId);
		} else {
			await startCamera();
		}
	});

	onDestroy(() => stopCamera());
</script>

<svelte:head>
	<title>Analisis Kematangan Pisang Cavendish</title>
	<link rel="icon" href="/logo.jpeg" />
</svelte:head>

<main
	class="flex min-h-screen flex-col items-center bg-gradient-to-br from-yellow-50 to-yellow-100 px-4 py-10"
>
	<h1 class="mb-8 text-center text-3xl font-bold text-yellow-800 md:text-4xl">
		🍌 Deteksi Kematangan Pisang Cavendish
	</h1>

	<div class="flex w-full max-w-6xl flex-col items-start justify-center gap-8 md:flex-row">
		<div class="flex flex-col items-center">
			<video
				bind:this={videoEl}
				autoplay
				playsinline
				muted
				class="w-full max-w-md rounded-2xl bg-black shadow-lg"
			></video>
			<canvas bind:this={canvasEl} class="hidden"></canvas>
		</div>

		<div class="flex w-full flex-col gap-4 rounded-2xl bg-white p-6 shadow-md md:w-96">
			<label class="flex flex-col text-sm font-medium text-gray-700">
				Pilih Kamera:
				<select
					bind:value={selectedDeviceId}
					on:change={onChangeDevice}
					class="mt-1 rounded-lg border border-gray-300 p-2 outline-none focus:ring-2 focus:ring-yellow-400"
				>
					{#if devices.length === 0}
						<option disabled>Memuat perangkat...</option>
					{/if}
					{#each devices as d}
						<option value={d.deviceId}>{d.label || `Camera ${d.deviceId}`}</option>
					{/each}
				</select>
			</label>

			<div class="flex flex-wrap gap-2">
				<button
					on:click={() => takePhoto()}
					disabled={taking}
					class="rounded-lg bg-yellow-500 px-4 py-2 text-white shadow transition hover:bg-yellow-600"
				>
					Ambil Foto
				</button>
				<button
					on:click={() => startCamera(null, 'environment')}
					class="rounded-lg bg-blue-500 px-4 py-2 text-white shadow transition hover:bg-blue-600"
				>
					Kamera Belakang
				</button>
				<button
					on:click={() => startCamera(null, 'user')}
					class="rounded-lg bg-blue-500 px-4 py-2 text-white shadow transition hover:bg-blue-600"
				>
					Kamera Depan
				</button>
				<button
					on:click={stopCamera}
					class="rounded-lg bg-red-500 px-4 py-2 text-white shadow transition hover:bg-red-600"
				>
					Stop Kamera
				</button>
			</div>

			{#if photoDataUrl}
				<div class="mt-4">
					<p class="mb-2 font-semibold text-gray-700">📷 Hasil Foto:</p>
					<img src={photoDataUrl} alt="Preview foto" class="max-w-full rounded-lg shadow-md" />
					<div class="mt-3 flex gap-2">
						<button
							on:click={downloadPhoto}
							class="flex-1 rounded-lg bg-green-500 px-4 py-2 text-white shadow transition hover:bg-green-600"
						>
							Download
						</button>
						<button
							on:click={uploadPhoto}
							class="flex-1 rounded-lg bg-purple-500 px-4 py-2 text-white shadow transition hover:bg-purple-600"
						>
							Upload
						</button>
					</div>
				</div>
			{/if}
		</div>
	</div>
</main>
