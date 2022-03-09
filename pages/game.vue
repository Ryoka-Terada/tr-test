<template>
  <div class="textLight">
    <v-row>
      <p class="explain">頭の休憩にどうぞ……</p>
    </v-row>
    <v-row justify="center">
      <v-card-title>
        <h2>ネオ○×ゲーム</h2>
      </v-card-title>
      <v-card-text class="d-flex flex-row-reverse">ネオ○×ゲームとは(説明)</v-card-text>
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
            ></v-text-field>
          </v-col>
          <span>VS</span>
          <v-col>
            <v-text-field
              v-model="playerName2"
              label="プレイヤー２(後攻)"
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
            ></v-select>
          </v-col>
          <v-col class="d-flex flex-row-reverse align-end">
            <v-btn color="primary">ゲーム開始</v-btn>
          </v-col>
        </v-row>
      </v-card-actions>
    </v-card>
    <br />
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
.textLight{
  color: #C2B2B4;
}
.areaSpace{
  padding: 13px;
}
</style>
