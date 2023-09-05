<template>
  <div class="w-[80%] mx-auto mt-10" id="projects">
    <h1 class="projectsHeading">Latest Projects</h1>
    <FilterNav
      :selectedCategory="selectedCategory"
      @filter-projects="filterProjects"
    ></FilterNav>

    <div class="projectsSec">
      <div class="responsiveLayout">
        <div
          class="p-4"
          v-for="completedProject in filteredProjects"
          :key="completedProject.id"
        >
          <div class="projectCard" data-aos="fade-up" data-aos-duration="1000">
            <img
              class="w-full h-48 object-cover"
              :src="completedProject.imageUrl"
              alt="Project Image"
            />
            <div class="p-4">
              <h2 class="text-xl font-semibold mb-2">
                {{ completedProject.project_name }}
              </h2>
              <p class="text-gray-600 mb-3">
                Location: {{ completedProject.location }}
              </p>
              <p class="text-gray-800 mb-4">
                {{ completedProject.description }}
              </p>
              <div class="flex items-center justify-between">
                <p class="text-sm text-gray-500">
                  Completion Date: {{ completedProject.completed_date }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, onMounted, ref } from "vue";
import FilterNav from "../components/filterNav";
import { collection, getDocs, orderBy } from "firebase/firestore";
import db from "@/firebase/init";
export default {
  components: {
    FilterNav,
  },
  setup() {
    let allcompletedProjects = ref([]);
    let selectedCategory = ref("all");
    //getAllEquipments
    onMounted(async () => {
      const querySnapshot = await getDocs(
        collection(db, "completed_projects"),
        orderBy("completed_date", "desc")
      );
      querySnapshot.forEach((doc) => {
        //change time stamp to string
        const completedDateTimestamp = doc.data().completed_date;
        const completedDate = completedDateTimestamp.toDate();
        const formattedDate = completedDate.toLocaleDateString("en-US");
        //change time stamp to string
        const ProjectData = {
          id: doc.id,
          ...doc.data(),
          completed_date: formattedDate,
        };
        allcompletedProjects.value.push(ProjectData);
      });
    });

    //filter local, oversea
    let filterProjects = (category) => {
      selectedCategory.value = category;
    };

    const filteredProjects = computed(() => {
      if (selectedCategory.value === "all") {
        // If 'All' is selected, return allcompletedProjects as is
        return allcompletedProjects.value;
      } else {
        // Filter projects based on the selected category
        return allcompletedProjects.value.filter(
          (project) => project.local_oversea === selectedCategory.value
        );
      }
    });
    return {
      allcompletedProjects,
      selectedCategory,
      filterProjects,
      filteredProjects,
    };
  },
};
</script>

<style>
.projectsHeading {
  @apply text-center text-2xl text-[#D98106];
}
.responsiveLayout {
  @apply grid grid-cols-1 md:grid-cols-3 gap-4;
}
.projectCard {
  @apply max-w-md mx-auto bg-white shadow-lg rounded-lg overflow-hidden;
}
</style>