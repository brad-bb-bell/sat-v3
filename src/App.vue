<template>
  <div class="container mx-auto py-4 px-4">
    <Section>
      <h1 class="text-xl tracking-widest">Simple Activity Tracker</h1>
    </Section>
  </div>
</template>
<script>
import Section from "./components/Section.vue";
import Login from "./components/Login.vue";

export default {
  components: {
    Section,
    Login,
  },
  data() {
    return {
      isLoggedIn: false,
      showLogin: false,
      showSignup: false,
    };
  },
};
</script>
