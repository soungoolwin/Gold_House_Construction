<template>
  <div class="contactusSection">
    <!-- Contact Us -->
    <div class="contactUs">
      <h2 class="contactusHeading">Contact Us</h2>
      <a href="https://goo.gl/maps/a9wotcA555WbCvJb8">
        <p>
          Our Address is <br />
          <span class="hover:text-blue-500">
            No(3), Thumingalar Road, Thumingalar Housing, Thingangyun T/S,
            Yangon, Myanmar
          </span>
        </p>
      </a>
      <p class="my-4">
        Phone :
        <a href="tel:+95 9759528528" class="hover:text-blue-500"
          >+95 9759528528</a
        >
      </p>
      <div class="pb-5">
        <p class="socials">
          <i class="fa-brands fa-facebook"></i>
          <i class="fa-brands fa-instagram"></i>
          <i class="fa-brands fa-linkedin"></i>
        </p>
      </div>
    </div>

    <!-- Form -->
    <div class="contactusForm">
      <form @submit.prevent="saveClientMessage">
        <p class="text-xl my-5">
          Please Fill your contact info. We will contact you back ASAP.
        </p>
        <label for="email" class="block mb-2">Your Email Address:</label>
        <input
          type="email"
          id="email"
          name="email"
          v-model="email"
          class="w-full px-3 py-2 border rounded"
          required
        />

        <label for="email" class="block mb-2">Write message to us:</label>

        <textarea
          name="message"
          v-model="message"
          class="w-full px-3 py-2 border rounded"
          id="message"
          cols="30"
        ></textarea>
        <p class="text-red-500">{{ errmsg }}</p>
        <button
          type="submit"
          class="mt-8 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
        >
          Send
        </button>
      </form>
    </div>
  </div>

  <!-- Map -->
  <div class="mapCard">
    <iframe
      src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3819.0115677829485!2d96.18556697533677!3d16.82578221874057!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x30c193721adb815f%3A0x4553ec00e9a37552!2sThumingalar%20Housing!5e0!3m2!1sth!2sth!4v1692451701623!5m2!1sth!2sth"
      width="100%"
      height="450"
      style="border: 0"
      allowfullscreen=""
      loading="lazy"
      referrerpolicy="no-referrer-when-downgrade"
    ></iframe>
  </div>

  <!-- Popup -->
  <div
    v-if="showPopup"
    class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
  >
    <div class="bg-white p-6 rounded shadow-lg">
      <h2 class="text-xl font-semibold mb-4">
        <button @click="showPopup = false" class="">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </h2>
      <p class="mb-4">Our Team will contact you ASAP!!</p>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import db from "../firebase/init";
import { collection, addDoc } from "firebase/firestore";

export default {
  setup() {
    let email = ref("");
    let message = ref("");
    let showPopup = ref(false);
    let errmsg = ref("");

    let saveClientMessage = async () => {
      try {
        const docRef = await addDoc(collection(db, "clients"), {
          email: email.value,
          message: message.value,
        });
        if (docRef.id) {
          showPopup.value = true;
          email.value = "";
        }
      } catch (e) {
        errmsg.value = "Error sending message";
      }
    };

    return { email, message, saveClientMessage, showPopup, errmsg };
  },
};
</script>

<style>
/* contactus section */
.contactusSection {
  @apply grid grid-cols-1 md:grid-cols-2 gap-4 mt-[8rem];
}
.contactUs {
  @apply p-4 w-[70%] mx-auto;
}
.contactusHeading {
  @apply text-center text-xl my-5;
}
.contactusForm {
  @apply p-4 w-[70%] mx-auto;
}
.socials i {
  @apply px-1;
}

/* map */
.mapCard {
  @apply w-[80%] mx-auto my-10;
}
</style>
