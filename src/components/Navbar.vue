<template>
  <div class="navFrame">
    <!-- Left section (logo) -->
    <div>
      <img src="../../public/img/Logo.png" alt="Logo" class="logo" />
    </div>
    <!-- Left section (logo) -->

    <!-- Right section (navigation list) -->
    <nav class="hidden sm:block">
      <ul class="nav-ul">
        <li class="navList"><router-link to="/">Home</router-link></li>
        <li class="navList">
          <router-link :to="{ name: 'projects' }">Projects</router-link>
        </li>
        <li class="navList">
          <router-link :to="{ name: 'equipments' }">Equipments</router-link>
        </li>
        <li class="navList">
          <router-link :to="{ name: 'contact' }">Contact</router-link>
        </li>
      </ul>
    </nav>
    <!-- Right section (navigation list) -->

    <!-- For small screen -->
    <button @click="toggleMenu()" class="toggleBtn py-2">
      <i class="fa-solid fa-bars"></i>
    </button>
  </div>
  <div v-if="showMobileMenu" class="md:hidden mt-[6rem]">
    <div class="mobileNav">
      <router-link to="/" class="dropdownLink">Home</router-link>
      <router-link :to="{ name: 'projects' }" class="dropdownLink"
        >Projects</router-link
      >
      <router-link :to="{ name: 'equipments' }" class="dropdownLink"
        >Equipments</router-link
      >
      <router-link :to="{ name: 'contact' }" class="dropdownLink"
        >Contact</router-link
      >
    </div>
  </div>
  <!-- For small screen -->
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    let showMobileMenu = ref(false);
    let toggleMenu = () => {
      showMobileMenu.value = !showMobileMenu.value;
    };
    return {
      showMobileMenu,
      toggleMenu,
    };
  },
};
</script>

<style>
.custom-active-class {
  color: black;
  border: 1px solid black; /* Set the border width and color */

  padding: 5px;
  border-radius: 20px;
}
.nav-ul {
  @apply flex items-center space-x-10;
}
.navFrame {
  @apply flex justify-between items-center px-10 py-2 bg-[#ffff] fixed top-0 w-full bg-white shadow-lg;
  z-index: 999;
}
.navList {
  @apply text-black cursor-pointer;
}
.logo {
  width: 90px;
  height: 50px;
}

/* Mobile Navigation */
.mobileNav {
  @apply px-2 pt-2 py-3 space-y-1 sm:px-3;
}
.dropdownLink {
  @apply text-gray-500  block px-3 py-2 rounded-md text-base font-medium;
}
.toggleBtn {
  float: right;
  @apply sm:hidden;
}
</style>
