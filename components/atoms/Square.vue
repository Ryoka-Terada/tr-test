<template>
  <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="value!=''" @click="select">
    {{ value }}
  </v-btn>
</template>

<script lang="ts">
import { Vue, Component, Prop } from "nuxt-property-decorator";

@Component
export default class GameBoard extends Vue {
  @Prop({ type: String, required: true })
  value: String;

  select(){
    this.$emit("changePlayer");
  }
}
</script>

<style scoped>
.gameButtom {
  background-color: azure;
}
</style>
