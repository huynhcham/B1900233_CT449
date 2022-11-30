<script>
import AppHeader from "@/components/AppHeader.vue";

export default {
	components: {
		AppHeader,
	},
};
</script>

<template>
	
	<div id="app" >
		<div
  class="bg-image"
  style="
    background-image: url('https://i.pinimg.com/originals/47/cf/c9/47cfc99b043db95e8a236b22c1a5d454.jpg');
    height: 150vh;
  "
>
		
		<AppHeader />
		
			<div class="container mt-5
			">
				<router-view />
			</div>
		</div>
		
</div>
</template>

<style>
.page {
	max-width: 500px;
	margin: auto;
}

.swal2-popup {
	padding: 0 0 1rem 0;
}

.swal2-close {
	padding: 0;
}

.swal2-popup.swal2-toast .swal2-title {
	margin: 1rem 1rem 0 1rem;
}

.swal2-popup.swal2-toast .swal2-html-container {
	margin: 1rem;
}
</style>
