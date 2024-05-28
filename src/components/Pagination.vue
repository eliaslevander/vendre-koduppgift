<script setup lang="ts">
import { Icon } from "@iconify/vue";

const emit = defineEmits(["setPage"]);

const props = defineProps<{
  currentPage: number;
}>();

// Emit page number to EmployeesView

const handleClick = (page: number) => {
  emit("setPage", page);
};

// Return boolean if the current page is equal to the currentPage prop

const isActive = (page: number) => {
  return page === props.currentPage ? "active" : "";
};
</script>

<template>
  <div id="wrapper">
    <nav class="pagination-container" aria-label="Pagination">
      <button
        class="pagination-button"
        @click="handleClick(props.currentPage - 1)"
        :disabled="props.currentPage === 1"
        aria-label="Previous page"
      >
        <Icon icon="mdi:chevron-left" width="40" />
      </button>
      <span
        class="page-selector"
        :class="isActive(1)"
        @click="handleClick(1)"
        aria-label="Go to page 1"
        >1</span
      >
      <span
        class="page-selector"
        :class="isActive(2)"
        @click="handleClick(2)"
        aria-label="Go to page 2"
        >2</span
      >
      <button
        class="pagination-button"
        @click="handleClick(props.currentPage + 1)"
        :disabled="props.currentPage === 2"
        aria-label="Next page"
      >
        <Icon icon="mdi:chevron-right" width="40" />
      </button>
    </nav>
  </div>
</template>

<style scoped>
#wrapper {
  width: 100%;
  margin: 2vh 0;
}

.pagination-container {
  width: 40vw;
  margin: 0 auto;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  @media screen and (min-width: 769px) {
    width: 20vw;
  }

  @media screen and (min-width: 1440px) {
    width: 10vw;
  }
}

.pagination-button {
  border: none;
  background-color: transparent;
  cursor: pointer;
  width: 40px;
  height: 40px;
  color: #000;
}

.pagination-button:disabled {
  color: #ddd;
}

.page-selector {
  cursor: pointer;
  font-size: var(--p-mobile);
  border-radius: 0.5rem;
  width: 2rem;
  height: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  @media screen and (min-width: 769px) {
    font-size: var(--p-mobile);
  }
}

.page-selector.active {
  background-color: var(--color-primary);
  box-shadow: 0px 0px 10px var(--color-primary);
  color: #fff;
  text-decoration: underline;
}
</style>
