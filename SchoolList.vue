<template>
  <div class="school-list-container">
    <h1>School List</h1>
    <div v-if="loading" class="loading">Loading...</div>
    <div v-if="error" class="error">{{ error }}</div>
    <input
      type="text"
      v-model="searchQuery"
      placeholder="Search for a school..."
      @input="filterSchools"
      class="search-input"
    />
    <div class="school-list">
      <div
        v-for="school in filteredSchools"
        :key="school.id"
        class="school-item"
      >
        <div class="school-image">
          <img :src="school.logo_url" alt="School Logo" />
        </div>
        <div class="school-info">
          <h2 class="school-name">{{ school.school_name }}</h2>
          <p class="school-location">
            {{ school.city_name }}, {{ school.state_name }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      schools: [],
      filteredSchools: [],
      loading: false,
      error: null,
      searchQuery: "",
    };
  },
  methods: {
    async fetchSchools() {
      this.loading = true;
      this.error = null;

      try {
        const response = await fetch(
          "http://api.devharlemwizardsinabox.com/campaign/campaign_school_list/"
        );

        if (!response.ok) {
          throw new Error("Error fetching data.");
        }
        const data = await response.json();
        if (data.success) {
          this.schools = data.school_list;
          this.filteredSchools = data.school_list;
        } else {
          this.error = "No results found.";
        }
      } catch (error) {
        this.error = error.message;
      } finally {
        this.loading = false;
      }
    },
    filterSchools() {
      if (this.searchQuery) {
        this.filteredSchools = this.schools.filter((school) =>
          school.school_name
            .toLowerCase()
            .includes(this.searchQuery.toLowerCase())
        );
      } else {
        this.filteredSchools = this.schools;
      }
    },
  },
  mounted() {
    this.fetchSchools();
  },
};
</script>

<style scoped>
.school-list-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.loading {
  font-size: 1.2em;
  color: #007bff;
}

.error {
  color: red;
  font-weight: bold;
  margin-bottom: 20px;
}

.search-input {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1em;
}

.school-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.school-item {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 15px;
  display: flex;
  align-items: center;
  transition: background-color 0.3s, box-shadow 0.3s;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.school-item:hover {
  background-color: #f9f9f9;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.school-image {
  margin-right: 15px;
}

.school-image img {
  width: 80px;
  height: auto;
  border-radius: 4px;
}

.school-info {
  padding: 10px 0;
}

.school-name {
  font-size: 1.6em;
  margin: 0;
  color: #333;
}

.school-location {
  color: #777;
}

.school-description {
  margin-top: 5px;
  color: #555;
}
</style>
