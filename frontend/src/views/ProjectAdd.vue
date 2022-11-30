<template>
	<div class="page bg-light">
		<div>
			<h4 class="card-header bg-info text-white">Thêm bài hát mới</h4>
			<ProjectForm
				:project="{}"
				:resetAfterSubmit="false"
				@submit:project="saveProject"
			/>
		</div>
	</div>
</template>

<script>
import { swtoast } from "@/mixins/swal.mixin";
import ProjectForm from "@/components/ProjectForm.vue";
import ProjectService from "@/services/project.service";

export default {
	components: {
		ProjectForm,
	},
	methods: {
		async saveProject(data) {
			try {
				await ProjectService.create(data);
				swtoast.success({
					text: "Thêm bài hát mới thành công.",
					
				});
					this.$router.push({ name: "project" });
			} catch (error) {
				console.log(error);
				swtoast.error({
					text: "Đã xảy ra lỗi.",
				});
			}		
		},
	},
};
</script>
