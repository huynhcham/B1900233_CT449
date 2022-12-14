<template>
	<div class="page row">
		<div class="col-md-10">
			<InputSearch v-model="searchText" />
		</div>
		<div class="mt-3 col-md-6">
			<h4>
				Danh sách bài hát
				<i class="fa-solid fa-headphones"></i>
			</h4>
			<ProjectList
				v-if="filteredProjectsCount > 0"
				:projects="filteredProjects"
				v-model:activeIndex="activeIndex"
			/>
			<p v-else>Không có nào trong danh sách.</p>
			<div class="mt-3 row justify-content-around align-items-center">
				<button class="btn btn-sm btn-info alert" @click="refreshList()">
					<i class="fas fa-redo"></i> Làm mới
				</button>

				<button class="btn btn-sm btn-success alert" @click="goToAddProject">
					<i class="fas fa-plus"></i> Thêm mới
				</button>

				<button
					class="btn btn-sm btn-danger alert"
					@click="removeAllProjects"
				>
					<i class="fas fa-trash"></i> Xóa tất cả
				</button>
			</div>
		</div>
		<div class="mt-3 col-md-6">
			<div v-if="activeProject">
				<h4 class="card-header bg-secondary text-white">
					Chi tiết bài hát
						<i class="fa-solid fa-music"></i>
				</h4>
				
					<ProjectCard :project="activeProject" />
					<router-link 
						:to="{
							name: 'project.edit',
							params: { id: activeProject._id },
						}"
					>
						<span class="mt-10 badge badge-warning">
							<i class="fas fa-edit"></i> Chỉnh Sửa</span
						>
					</router-link>
				
			</div>
		</div>
	</div>
</template>

<script>
import { swalert } from "@/mixins/swal.mixin";
import ProjectCard from "@/components/ProjectCard.vue";
import InputSearch from "@/components/InputSearch.vue";
import ProjectList from "@/components/ProjectList.vue";
import ProjectService from "@/services/project.service";

export default {
	components: {
		ProjectCard,
		InputSearch,
		ProjectList,
	},
	data() {
		return {
			projects: [],
			activeIndex: -1,
			searchText: "",
		};
	},
	watch: {
		searchText() {
			this.activeIndex = -1;
		},
	},
	computed: {
		projectStrings() {
			return this.projects.map((project) => {
				const { name, lyrics, link} = project;
				return [name, lyrics, link].join("");
			});
		},
		filteredProjects() {
			if (!this.searchText) return this.projects;
			return this.projects.filter((_project, index) =>
				this.projectStrings[index].includes(this.searchText)
			);
		},
		activeProject() {
			if (this.activeIndex < 0) return null;
			return this.filteredProjects[this.activeIndex];
		},
		filteredProjectsCount() {
			return this.filteredProjects.length;
		},
	},
	methods: {
		async retrieveProjects() {
			try {
				this.projects = await ProjectService.getAll();
				this.projects.sort((current, next) =>
					current.name.localeCompare(next.name)
				);
			} catch (error) {
				console.log(error);
			}
		},

		refreshList() {
			this.retrieveProjects();
			this.activeIndex = -1;
		},

		async removeAllProjects() {
			swalert
				.fire({
					title: "Xóa tất cả bài hát",
					icon: "warning",
					text: "Bạn muốn xóa tất cả bài hát?",
					showCloseButton: true,
					showCancelButton: true,
				})
				.then(async (result) => {
					if (result.isConfirmed) {
						try {
							await ProjectService.deleteAll();
							this.refreshList();
						} catch (error) {
							console.log(error);
						}
					}
				});
		},

		goToAddProject() {
			this.$router.push({ name: "project.add" });
		},
	},
	created() {
		this.refreshList();
	},
};
</script>

<style scoped>
.page {
	text-align: left;
	max-width: 750px;
}
</style>
