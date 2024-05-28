<script setup lang="ts">
// ref for state management, onMounted to do stuff when view is mounted
import { ref, onMounted } from "vue";

// -----------------Axios for simple data fetching-----------------
import axios from "axios";

// -----------------Components-----------------

import EmployeeCard from "../components/EmployeeCard.vue";
import Pagination from "../components/Pagination.vue";

// -----------------Interface-----------------

import Employee from "../interfaces/Employee";

// -----------------Set refs-----------------

// Using the Employee interface as a generic for the employees state

const employees = ref<Employee[]>([]);

// ----------------- Current page is set to start at 1 -----------------
const currentPage = ref(1);

// Fetch the data using template strings to insert page parameter

const fetchEmployees = async (page: number, updatePage = true) => {
  try {
    const response = await axios.get(
      `https://reqres.in/api/users?page=${page}`
    );
    employees.value = await response.data.data;
    if (updatePage) {
      currentPage.value = page;
    }
  } catch (error) {
    console.error("Could not fetch employee data", error);
  }
};

// Handler for emitting the page number that is sent to the pagination

const handleSetPage = (page: number) => {
  fetchEmployees(page);
};

// Fetch employees on page one on mount

onMounted(() => {
  fetchEmployees(currentPage.value);
});
</script>

<template>
  <div>
    <h1 class="main-heading">Våra medarbetare</h1>
    <p class="main-text">
      Välkommen till vårt team på Reqres! Nedan hittar du våra namn och
      e-postadresser för enkel kontakt.
    </p>
    <section id="employee-container">
      <!-- Iterate through all the employees with v-for and send the values to the component as props -->
      <!-- Send index as a tabindex prop, needed to be able to target the mail using tab -->
      <EmployeeCard
        v-for="(employee, index) in employees"
        :key="employee.id"
        :="{
          firstName: employee.first_name,
          lastName: employee.last_name,
          avatar: employee.avatar,
          id: employee.id,
          email: employee.email,
          tabindex: index,
        }"
      />
    </section>
    <!-- Use emitted value from Pagination to set the page, send the current page as
        a prop to pagination so that it can tell which page is active
     -->
    <Pagination @set-page="handleSetPage" :currentPage="currentPage" />
  </div>
</template>

<style scoped>
.main-heading {
  margin: 2vh auto;
  text-align: center;
  font-size: var(--h1-mobile);
  @media screen and (min-width: 769px) {
    font-size: var(--h1-desktop);
  }
}

.main-text {
  text-align: center;
  font-size: var(--p-mobile);
  width: 85vw;
  margin: 0 auto 2vh;
  @media screen and (min-width: 769px) {
    font-size: var(--p-desktop);
  }

  @media screen and (min-width: 1025px) {
    width: 40vw;
  }
}

#employee-container {
  margin-top: 2vh;
  display: grid;
  grid-template-columns: repeat(2, 1fr);

  grid-gap: 2vw;
  padding: 2vw;

  margin: 0 auto 2vh;
  max-width: 1000px;
  @media screen and (min-width: 769px) {
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, auto);
  }

  @media screen and (min-width: 1025px) {
    width: 75vw;
  }
  @media screen and (min-width: 1367px) {
    width: 60vw;
  }
}
</style>
