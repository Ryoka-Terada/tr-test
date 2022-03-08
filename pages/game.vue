<template>
  <div>
    <v-row>
      <p class="explain">頭の休憩にどうぞ……</p>
    </v-row>
    <v-row justify="center">
      <v-card-title><h2>ネオ○×ゲーム</h2></v-card-title>
    </v-row>
    <v-row justify="center">
      <p class="explain">マスに表示されるのはプレイヤー名の最初の２文字です</p>
    </v-row>
    <v-row justify="center">
      <v-col>
        <v-text-field
          v-model="playerName1"
          label="プレイヤー名を入力してください(先攻)"
        ></v-text-field>
      </v-col>
      <span>VS</span>
      <v-col>
        <v-text-field
          v-model="playerName2"
          label="プレイヤー名を入力してください(後攻)"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" sm="4" md="2">
        <v-select
          v-model="boardSize"
          :items="choiseSize"
          label="マス目の数"
        ></v-select>
      </v-col>
    </v-row>
    <v-row>
      <GameBoard :boardsize="boardSize" :playername1="playerName1" :playername2="playerName2" />
    </v-row>
    <br />
  </div>
</template>

<script lang="ts">
import { Vue, Component, Watch } from "nuxt-property-decorator";

@Component
export default class test extends Vue {
  // ゲームボードのサイズを管理
  boardSize: number = 3;
  choiseSize: number[] = [3,4,5,6,7];
  // プレイヤー名
  playerName1: string = "〇";
  playerName2: string = "×";

  // プレイヤー名は入力された最初の2文字
  @Watch("playerName1", {deep:true})
  @Watch("playerName2", {deep:true})
  cutName(){
    this.playerName1 = this.playerName1.substring(0,2);
    this.playerName2 = this.playerName2.substring(0,2);
  }
}
</script>


<style scoped>
.gameButtom {
  background-color: azure;
}
.center {
  margin: center;
}
span {
  display: inline-flex;
  margin: 20px;
  margin-top: 20px;
}
p.explain{
  font-size: 15px;
  padding-left: 12px;
}
.col{
  padding-bottom: 1px;
}
</style>
