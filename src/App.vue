<template>
  <the-header title="Flights"></the-header>
  <add-flight></add-flight>
</template>

<script>
import TheHeader from "./components/layouts/TheHeader.vue";

import AddFlight from "./components/AddFlight.vue";

export default {
  components: { TheHeader, AddFlight },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

* {
  box-sizing: border-box;
}

html {
  font-family: "Roboto", sans-serif;
}

body {
  margin: 0;
}
</style>
