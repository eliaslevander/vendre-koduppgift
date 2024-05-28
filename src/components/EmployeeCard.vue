<script setup lang="ts">
defineProps({
  firstName: String,
  lastName: String,
  avatar: String,
  email: String,
  id: Number,
});
</script>

<template>
  <div class="employee-card">
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
      <a
        class="mail-link"
        :title="`Skriv ett mail till ${firstName} ${lastName}`"
        :href="`mailto:${email}`"
        >{{ email }}</a
      >
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
  width: 93%;
  height: 93%;
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
  background-color: #5333ed;
  box-shadow: 0px 0px 20px #7600fc;
  border-radius: 50%;
  opacity: 0.5;
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
  @media screen and (min-width: 1025px) {
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
  @media screen and (min-width: 769px) {
    font-size: 1rem;
  }
}
</style>
