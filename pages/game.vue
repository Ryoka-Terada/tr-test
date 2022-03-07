<template>
  <div>
    <v-row justify="center">
      <v-card-title>○×ゲーム</v-card-title>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" sm="4" md="2">
        <v-select
          v-model="boardSize"
          :items="choiseSize"
        ></v-select>
      </v-col>
    </v-row>
    <v-row>
      <GameBoard :boardsize="boardSize" />
    </v-row>
    <br />
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "nuxt-property-decorator";

@Component
export default class test extends Vue {
  // ゲームボードのサイズを管理
  boardSize: number = 3;
  choiseSize: number[] = [3,4,5,6,7];
}
</script>


<style scoped>
.gameButtom {
  background-color: azure;
}
.center {
  margin: center;
}
</style>

