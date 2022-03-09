<template>
  <div class="textLight">
    <v-row>
      <p class="explain">頭の休憩にどうぞ……</p>
    </v-row>
    <v-row justify="center">
      <v-card-title>
        <h2>ネオ○×ゲーム</h2>
      </v-card-title>
      <v-card-text class="d-flex flex-row-reverse">
        <v-tooltip top>
          <template #activator="{ on, attrs }">
            <span
              v-bind="attrs"
              v-on="on"
            ><v-icon color="accent">mdi-help-circle-outline</v-icon>&nbsp;ネオ○×ゲームとは</span>
          </template>
          <span>
            通常の○×ゲームとは異なりポイント制になります。<br />ビンゴなら2ポイント、リーチなら1ポイント入ります。<br /><br />
            4×4以降のボードではウィザードマスがランダムで出現します。<br />ウィザードマスは後攻プレイヤーのマスとして機能します。
          </span>
        </v-tooltip>
      </v-card-text>
    </v-row><br />
    <v-card class="areaSpace">
      <v-row>
        <v-col>
          <p class="explain">マスに表示されるのはプレイヤー名の最初の２文字です</p>
        </v-col>
      </v-row>
      <v-card-actions>
        <v-row justify="center">
          <v-col>
            <v-text-field
              v-model="playerName1"
              label="プレイヤー１(先攻)"
              :disabled="isGameStart"
            ></v-text-field>
          </v-col>
          <span>VS</span>
          <v-col>
            <v-text-field
              v-model="playerName2"
              label="プレイヤー２(後攻)"
              :disabled="isGameStart"
            ></v-text-field>
          </v-col>
        </v-row>
      </v-card-actions>
      <v-card-actions>
        <v-row>
          <v-col>
            <v-select
              v-model="boardSize"
              :items="choiseSize"
              label="マス目の数"
              :disabled="isGameStart"
            ></v-select>
          </v-col>
          <v-col class="d-flex flex-row-reverse align-end">
            <v-btn v-if="isGameStart" color="primary" @click="game">リセット</v-btn>
            <v-btn v-else color="primary" @click="game">ゲーム開始</v-btn>
          </v-col>
        </v-row>
      </v-card-actions>
    </v-card>
    <br />
    <v-row>
      <GameBoard :boardsize="boardSize" :playername1="playerName1" :playername2="playerName2" :isgamestart="isGameStart" />
    </v-row>
    <br />
    <v-icon color="primary">mdi-arrow-down-thin</v-icon>
    <p class="hiddenText">あえてルールの抜け道を残しています。プレイヤー名をひねると面白い動きをするかもしれません。</p>
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
  // ゲーム開始フラグ
  isGameStart: Boolean = false;

  // プレイヤー名は入力された最初の2文字
  @Watch("playerName1", {deep:true})
  @Watch("playerName2", {deep:true})
  cutName(){
    this.playerName1 = this.playerName1.substring(0,2);
    this.playerName2 = this.playerName2.substring(0,2);
  }

  game(){
    this.isGameStart = !this.isGameStart;
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
.textLight{
  color: #C2B2B4;
}
.areaSpace{
  padding: 13px;
}
.hiddenText{
  color: #53687e81;
}
</style>
