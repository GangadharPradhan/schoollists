<template>
  <v-app>
    <v-main>
      <SchoolList/>
    </v-main>
  </v-app>
</template>

<script>
import SchoolList from './components/SchoolList.vue';
export default {
  name: 'App',
  components: {
    SchoolList
  },
};
</script>
