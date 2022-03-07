<template>
  <v-container>
    <v-row justify="center">
      <h5 v-if="player">Next player is 〇</h5>
      <h5 v-if="!player">Next player is ×</h5>
    </v-row>
    <v-row
      v-for="i of boardsize" :key="i"
      justify="center"
    >
      <Square
        v-for="j of boardsize" :key="j"
        :value="status[j+(i*boardsize)-boardsize-1]"
        @changePlayer="changePlayer(j+(i*boardsize)-boardsize-1)"
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
  // プレイヤーの手番を管理
  player: Boolean = true;

  created() {
    this.makeStatusArray();
  }

  @Watch("boardsize", {deep:true})
  makeStatusArray(){
    this.status = [];
    for (let i = 0; i < this.boardsize*this.boardsize; i++) {
      this.status.push("");
    }
  }

  changePlayer(num: number){
    this.player? this.status.splice(num,1,"〇") : this.status.splice(num,1,"×");
    this.player = !this.player;
  }
}
</script>
