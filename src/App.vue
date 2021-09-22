<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h3>School List</h3>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <School />
      <Student />
      <!-- <SchoolMethods/> -->
    </v-main>
  </v-app>
</template>

<script>
//import School from './components/School';
// import SchoolMethods from './components/SchoolMethods';
import School from "./components/School";
import Student from "./components/Student";

export default {
  name: "App",

  components: {
    School,
    Student,
    // SchoolMethods,
  },

  data: () => ({
    //
  }),
};
</script>
