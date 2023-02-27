<template>
  <div class="edits">
    <v-card :loading="loading">
      <template v-slot:loader="{ isActive }">
        <v-progress-linear
          :active="isActive"
          color="deep-purple"
          height="4"
          indeterminate
        ></v-progress-linear>
      </template>

      <v-card-title> Edits </v-card-title>

      <v-textarea
        label="Input"
        v-model="input"
        clearable
        clear-icon="mid-close-circle"
        class="mx-4"
      ></v-textarea>

      <v-textarea
        label="Instruction"
        v-model="instruction"
        clearable
        clear-icon="mid-close-circle"
        class="mx-4"
        row="2"
      ></v-textarea>

      <v-card-actions>
        <v-btn @click="edit" color="orange"> Generate </v-btn>
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
      input: "",
      instruction: "",

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
    async edit() {
      this.loading = true;
      const response = await this.openai
        .createEdit({
          model: this.model,
          input: this.input,
          instruction: this.instruction,
          n: 1,
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
.edits {
  width: 70%;
}
</style>
