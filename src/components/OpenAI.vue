<template>
  <div class="openai d-flex flex-column align-center mt-8 ml-64 mr-64">
    <div class="apiKey mb-8">
      <!-- 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 -->
      <v-text-field
        density="compact"
        variant="solo"
        label="Input Your API Key For OpenAI"
        single-line
        hide-details
        v-model="apiKey"
        type="password"
      ></v-text-field>
    </div>
    <div class="model">
      <v-select
        :items="models"
        label="Select the model"
        density="comfortable"
        v-model="selected_model"
      ></v-select>
    </div>
    <Completions :openai="openai" :model="selected_model" class="mt-8"></Completions>
    <Edits :openai="openai" :model="selected_model" class="mt-8"></Edits>
    <Images :openai="openai" :model="selected_model" class="mt-8"></Images>
  </div>
</template>

<script>
import { Configuration, OpenAIApi } from "openai";
import Completions from "./Completions.vue";
import Edits from "./Edits.vue";
import Images from "./Images.vue";
export default {
  name: "OpenAI",
  data() {
    return {
      apiKey: "",
      openai: null,
      models: [],
      selected_model: "text-davinci-003",
    };
  },
  watch: {
    async apiKey() {
      const configuration = new Configuration({
        organization: "org-YNktsOiZLtNF3fjmRXL65ch8",
        apiKey: this.apiKey,
      });
      this.openai = new OpenAIApi(configuration);

      const response = await this.openai.listModels();
      response.data.data.forEach((item) => this.models.push(item.id));
    },
  },
  components: {
    Completions,
    Edits,
    Images
  },
  methods: {},
  mounted() {
    this.apiKey = "";
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.openai {
  width: 100%;
  height: 100%;
}
.apiKey,
.model {
  width: 70%;
}
</style>
