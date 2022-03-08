<template>
  <v-container>
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

  @Prop({ type: String, required: true })
  playername1: string;

  @Prop({ type: String, required: true })
  playername2: string;

  // 各マスのステータスを保持
  status: string[] = [];
  // プレイヤーの手番を管理
  turn: Boolean = true;
  nextPlayer: String = "";

  created() {
    this.makeStatusArray();
    this.nextPlayer = this.playername1;
  }

  // ステータス配列を再生成
  @Watch("boardsize", {deep:true})
  makeStatusArray(){
    this.status = [];
    for (let i = 0; i < this.boardsize*this.boardsize; i++) {
      this.status.push("");
    }
  }

  // ステータスを登録してターン変更
  changeTurn(num: number){
    this.turn? this.status.splice(num,1,this.playername1) : this.status.splice(num,1,this.playername2);
    this.turn = !this.turn;
    this.nextPlayer = this.turn? this.playername1 : this.playername2;
  }
}
</script>
