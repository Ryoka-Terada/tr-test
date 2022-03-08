<template>
  <v-container>
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
    <v-row>
      <p class="explain">マスに表示されるのはプレイヤー名の最初の２文字です</p>
    </v-row>
    <v-row justify="center">
      <h5>Next player is {{ nextPlayer }}</h5>
    </v-row>
    <v-row></v-row>
    <v-row
      v-for="i of boardsize" :key="i"
      justify="center"
    >
      <Square
        v-for="j of boardsize" :key="j"
        :value="status[j+(i*boardsize)-boardsize-1]"
        @changeTurn="changeTurn(j+(i*boardsize)-boardsize-1)"
      ></Square>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from "nuxt-property-decorator";
import Square from "./atoms/Square.vue";

@Component({
  components: {
    Square
  }
})
export default class GameBoard extends Vue {
  @Prop({ type: Number, required: true })
  boardsize: number;

  // 各マスのステータスを保持
  status: string[] = [];
  // プレイヤー名
  playerName1: string = "〇";
  playerName2: string = "×";
  // プレイヤーの手番を管理
  turn: Boolean = true;
  nextPlayer: String = this.playerName1;

  created() {
    this.makeStatusArray();
  }

  // ステータス配列を再生成
  @Watch("boardsize", {deep:true})
  makeStatusArray(){
    this.status = [];
    for (let i = 0; i < this.boardsize*this.boardsize; i++) {
      this.status.push("");
    }
  }

  // プレイヤー名は入力された最初の2文字
  @Watch("playerName1", {deep:true})
  @Watch("playerName2", {deep:true})
  cutName(){
    this.playerName1 = this.playerName1.substring(0,2);
    this.playerName2 = this.playerName2.substring(0,2);
  }

  // ステータスを登録してターン変更
  changeTurn(num: number){
    this.turn? this.status.splice(num,1,this.playerName1) : this.status.splice(num,1,this.playerName2);
    this.turn = !this.turn;
    this.nextPlayer = this.turn? this.playerName1 : this.playerName2;
  }
}
</script>

<style scoped>
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
