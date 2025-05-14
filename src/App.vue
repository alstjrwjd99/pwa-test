<template>
	<div class="container">
		<h1>📸 PWA Camera</h1>
		<video ref="video" autoplay playsinline></video>
		<button @click="takePhoto">📷 사진 찍기</button>
		<canvas ref="canvas" class="hide"></canvas>
		<div v-if="photo">
			<h2>사진 미리보기</h2>
			<img :src="photo" alt="captured" />
		</div>
	</div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const video = ref(null);
const canvas = ref(null);
const photo = ref(null);

onMounted(async () => {
	try {
		const stream = await navigator.mediaDevices.getUserMedia({
			video: { facingMode: { exact: "environment" } }, // 📸 후면 카메라
			audio: false,
		});
		video.value.srcObject = stream;
	} catch (e) {
		alert("카메라 접근 실패 😢");
	}
});

function takePhoto() {
	const context = canvas.value.getContext("2d");
	canvas.value.width = video.value.videoWidth;
	canvas.value.height = video.value.videoHeight;
	context.drawImage(video.value, 0, 0);
	photo.value = canvas.value.toDataURL("image/png");
}
</script>

<style>
.container {
	text-align: center;
	padding: 20px;
}
video {
	width: 100%;
	max-width: 360px;
	border-radius: 10px;
}
img {
	width: 100%;
	max-width: 360px;
	margin-top: 10px;
	border-radius: 10px;
}
.hide {
	display: none;
}
</style>
