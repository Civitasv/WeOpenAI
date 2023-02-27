<template>
  <v-app>
    <v-main>
      <OpenAI />
    </v-main>
  </v-app>
</template>

<script>
import OpenAI from "./components/OpenAI.vue";

export default {
  name: "App",

  components: {
    OpenAI,
  },

  data: () => ({
    //
  }),
};
</script>
