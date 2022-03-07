<template>
  <div>
    <v-row justify="center">
      <v-card-title>○×ゲーム</v-card-title>
    </v-row>
    <v-container>
      <v-row justify="center">
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[0]!=''" @click="select(0)">
          {{ status[0] }}
        </v-btn>
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[1]!=''" @click="select(1)">
          {{ status[1] }}
        </v-btn>
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[2]!=''" @click="select(2)">
          {{ status[2] }}
        </v-btn>
      </v-row>
      <v-row justify="center">
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[3]!=''" @click="select(3)">
          {{ status[3] }}
        </v-btn>
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[4]!=''" @click="select(4)">
          {{ status[4] }}
        </v-btn>
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[5]!=''" @click="select(5)">
          {{ status[5] }}
        </v-btn>
      </v-row>
      <v-row justify="center">
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[6]!=''" @click="select(6)">
          {{ status[6] }}
        </v-btn>
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[7]!=''" @click="select(7)">
          {{ status[7] }}
        </v-btn>
        <v-btn class="gameButtom" height="60" width="40" outlined tile color="accent" :disabled="status[8]!=''" @click="select(8)">
          {{ status[8] }}
        </v-btn>
      </v-row>
    </v-container>
  </div>
</template>

<script lang="ts">
// TODO: アトミック化する
import { Vue, Component } from "nuxt-property-decorator";

@Component
export default class test extends Vue {
  // 各マスのステータスを保持
  status: [String, String, String, String, String, String, String, String, String] = ["", "", "", "", "", "", "", "", ""];

  // プレイヤーの手番を管理
  player: Boolean = true;

  // 選択されたマスにデータを入れる
  select(num: number){
    // this.status[num] = "〇" だとデータ更新にならないため画面に反映されない
    this.player? this.status.splice(num,1,"〇") : this.status.splice(num,1,"×");
    this.player = !this.player;
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
</style>

