<template>
  <div class="completions">
    <v-card :loading="loading">
      <template v-slot:loader="{ isActive }">
        <v-progress-linear
          :active="isActive"
          color="deep-purple"
          height="4"
          indeterminate
        ></v-progress-linear>
      </template>

      <v-card-title> Completions </v-card-title>

      <v-textarea
        label="Text"
        v-model="prompt_text"
        clearable
        clear-icon="mid-close-circle"
        class="mx-4"
      ></v-textarea>

      <v-card-actions>
        <v-btn @click="completion" color="orange"> Generate </v-btn>
        <v-spacer></v-spacer>
        <v-btn
          :icon="showResult ? 'mdi-chevron-up' : 'mdi-chevron-down'"
          @click="showResult = !showResult"
        ></v-btn>
      </v-card-actions>
      <v-expand-transition>
        <div v-show="showResult">
          <v-divider></v-divider>
          <v-card-text>
            <div v-for="(item, index) in result" :key="index">
              {{ item.text }}
            </div>
          </v-card-text>
        </div>
      </v-expand-transition>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "OpenAI",
  data() {
    return {
      prompt_text: "",
      result: [],
      loading: false,
      showResult: false,
    };
  },
  props: {
    model: String,
    openai: Object,
  },
  methods: {
    async completion() {
      this.loading = true;
      const response = await this.openai
        .createCompletion({
          model: this.model,
          prompt: this.prompt_text,
          max_tokens: 7,
          temperature: 0,
        })
        .catch();
      this.result = response.data.choices;
      this.loading = false;
      this.showResult = true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.completions {
  width: 70%;
}
</style>
