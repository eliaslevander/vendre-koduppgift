<script setup lang="ts">
import { ref } from "vue";
import { Icon } from "@iconify/vue";
defineProps({
  firstName: String,
  lastName: String,
  avatar: String,
  email: String,
  id: Number,
  tabindex: Number,
});

// Handle email visibility

const isHovering = ref(false);

// Handle focus on tab

const handleFocus = () => {
  isHovering.value = true;
};

// Hande blur on tab leave

const handleBlur = () => {
  isHovering.value = false;
};
</script>

<template>
  <!-- Show email icon when not hovering, show email address when hovering -->
  <div
    class="employee-card"
    tabindex="0"
    :aria-label="`${firstName} ${lastName}`"
    @mouseover="isHovering = true"
    @mouseleave="isHovering = false"
    @focus="handleFocus"
    @blur="handleBlur"
  >
    <div class="image-container">
      <img
        class="image"
        :src="avatar"
        :title="`${firstName} ${lastName}`"
        :alt="`An image of ${firstName} ${lastName}`"
      />
    </div>
    <h3 class="name">{{ firstName }} {{ lastName }}</h3>

    <div class="mail-container">
      <!-- If hovering, render this with v-show -->
      <a
        v-if="isHovering"
        class="mail-link"
        :title="`Skriv ett mail till ${firstName} ${lastName}`"
        :href="`mailto:${email}`"
        >{{ email }}</a
      >
      <!-- If not hovering, show this -->

      <!-- <Icon v-if="!isHovering" icon="mdi:email" width="28" /> -->
      <a
        v-else
        class="mail-link"
        :aria-label="`Skriv ett mail till ${firstName} ${lastName}`"
        :title="`Skriv ett mail till ${firstName} ${lastName}`"
        :href="`mailto:${email}`"
        ><Icon icon="mdi:email" width="28"
      /></a>
    </div>
  </div>
</template>

<style scoped>
.image-container {
  width: 100%;
  aspect-ratio: 1;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.employee-card {
  border-radius: 1vh;
  min-height: 30vh;
}

.image {
  border-radius: 50%;
  padding: 5px;
  width: 90%;
  height: 90%;
  object-fit: cover;
  display: block;
  cursor: pointer;
}

.employee-card:hover > .image-container::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 94%;
  height: 94%;
  /* background-color: var(--color); */
  background: linear-gradient(
    -45deg,
    var(--color-secondary),
    var(--color-primary)
  );
  box-shadow: 0px 0px 20px var(--color-primary);
  border-radius: 50%;
  z-index: -1;
  transform: translate(-50%, -50%) rotate(0deg) translate(0px) rotate(0deg);
  animation: orbit 0.2s linear, breathe alternate 0.5s infinite;
}

@keyframes breathe {
  0% {
    height: 92%;
    width: 92%;
  }
  100% {
    height: 94%;
    width: 94%;
  }
}

@keyframes orbit {
  0% {
    transform: translate(-50%, -50%) rotate(0deg) translate(5px) rotate(0deg);
  }
  100% {
    transform: translate(-50%, -50%) rotate(360deg) translate(0px)
      rotate(-360deg);
  }
}

.name {
  text-align: center;
  font-weight: 700;
  font-size: var(--h3-mobile);
  @media screen and (min-width: 767px) {
    font-size: var(--h3-desktop);
  }
  @media screen and (min-width: 1366px) {
    font-size: var(--h3-mobile);
  }
  @media screen and (min-width: 1440px) {
    font-size: var(--h3-desktop);
  }
}

.contact {
  cursor: pointer;
  display: inline;
}

.mail-container {
  margin: 1vh 0;
  text-align: center;
}

.mail-link {
  color: #000;
  display: inline-block;

  font-size: 0.88rem;
  @media screen and (min-width: 767px) {
    font-size: var(--p-mobile);
  }
}
</style>
