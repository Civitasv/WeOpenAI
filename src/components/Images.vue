<template>
  <div class="images">
    <v-card :loading="loading">
      <template v-slot:loader="{ isActive }">
        <v-progress-linear
          :active="isActive"
          color="deep-purple"
          height="4"
          indeterminate
        ></v-progress-linear>
      </template>

      <v-card-title> Images </v-card-title>

      <v-textarea
        label="Prompt"
        v-model="prompt"
        clearable
        clear-icon="mid-close-circle"
        class="mx-4"
      ></v-textarea>

      <v-card-actions>
        <v-btn @click="images" color="orange"> Generate </v-btn>
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
              {{ item.url }}
              <v-img
                :width="256"
                :height="256"
                aspect-ratio="1/1"
                cover
                :src="item.url"
              ></v-img>
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
      prompt: "",

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
    async images() {
      this.loading = true;
      const response = await this.openai
        .createImage({
          prompt: this.prompt,
          n: 2,
          size: "256x256",
          response_format: "url",
        })
        .catch();
      this.result = response.data.data;
      this.loading = false;
      this.showResult = true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.images {
  width: 70%;
}
</style>
