<template>
  <span>
  <v-btn v-if="address!=wizard" class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="value!=''||!isgamestart" @click="select">
    {{ value }}
  </v-btn>
  <v-btn v-else class="gameButtom" height="60" width="40" outlined tile color="accent" disabled>
    <v-icon>mdi-wizard-hat</v-icon>
  </v-btn>
  </span>
</template>

<script lang="ts">
import { Vue, Component, Prop } from "nuxt-property-decorator";

@Component
export default class GameBoard extends Vue {
  @Prop({ type: String, required: true })
  value: String;

  @Prop({ type: Number, required: true })
  address: number;

  @Prop({ type: Number, required: true })
  wizard: number;

  @Prop({ type: Boolean, required: true })
  isgamestart: Boolean;

  select(){
    this.$emit("changeTurn");
    this.$emit("pointCheck");
  }
}
</script>

<style scoped>
.gameButtom {
  background-color: azure;
}
</style>
