<template>
  <v-container>
    <h4>{{ playername1 }}のポイント：{{ pointPlayer1 }}</h4>
    <h4>{{ playername2 }}のポイント：{{ pointPlayer2 }}</h4>
    <v-row justify="center">
      <h5>Next player is {{ nextPlayer }}</h5>
    </v-row>
    <v-row
      v-for="i of boardsize" :key="i"
      justify="center"
    >
      <Square
        v-for="j of boardsize" :key="j"
        :value="status[j+(i*boardsize)-boardsize-1]"
        :address="(j+(i*boardsize)-boardsize-1)"
        @changeTurn="changeTurn(j+(i*boardsize)-boardsize-1)"
        @pointCheck="pointCheck(j+(i*boardsize)-boardsize-1)"
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
  // プレイヤーのポイントを管理
  pointPlayer1: number = 0;
  pointPlayer2: number = 0;
  // ステータスから抽出して結果判定するための配列(結果判定配列)
  pointCheckArray: number[][] = [];
  // 選択されたマスを含む列を管理
  arraysInSelectedAddress: number[][];
  // 結果判定
  resultArrays: string[];
  resultArray: string[][];

  created() {
    this.generateStatusArray();
    this.generatePointCheckArray();
    this.nextPlayer = this.playername1;
  }

  // マス目の数が変更されたら各種データを再生成
  @Watch("boardsize", {deep:true})
  regenerateArrays(){
    this.generateStatusArray(); // ステータス配列
    this.generatePointCheckArray(); // 結果判定配列
    this.pointPlayer1 = 0; // プレイヤー１の得点
    this.pointPlayer2 = 0; // プレイヤー２の得点
  }

  // マスを選択されたら結果判定処理を行う
  pointCheck(selectedAddress: number){
    // ジョーカーマスを指す文字列
    const joker = "joker";
    // 変数を初期化
    this.resultArray = [];
    this.arraysInSelectedAddress = [];
    this.pointCheckArray.forEach(statusAddressArray => {
      // 各列のテータスを取得
      // 選択されたマスに関係あるビンゴ列を取得
      if(statusAddressArray.includes(selectedAddress)){
        this.arraysInSelectedAddress.push(statusAddressArray);
      }
    });
    this.arraysInSelectedAddress.forEach(arrayInSelectedAddress => {
      this.resultArrays = [];
      arrayInSelectedAddress.forEach(statusAddress =>{
        // ロックアイコンの個所はジョーカー扱い
        statusAddress===12? this.resultArrays.push(joker) : this.resultArrays.push(this.status[statusAddress]);
      })
      this.resultArray.push(this.resultArrays);
    })
    // リーチ＝１点、ビンゴ＝２点
    this.resultArray.forEach(res =>{
      // 封鎖セルはどっちのプレイヤーのマスにもなる
      if(res.includes(joker)){
        res[res.indexOf(joker)]=this.turn? this.playername2 : this.playername1;
      }
      if(!this.turn){
        if(res.filter(n =>n===this.playername1).length===this.boardsize-1){
          this.$toast.success(this.playername1+"が1点ゲット");
          this.pointPlayer1++;
        }else if(res.filter(n =>n===this.playername1).length===this.boardsize) {
          this.$toast.warning(this.playername1+"がビンゴ！2点ゲット");
          this.pointPlayer1+=2;
        }
      } else if(this.turn) {
        if(res.filter(n =>n===this.playername2).length===this.boardsize-1){
          this.$toast.success(this.playername2+"がポイントゲット");
          this.pointPlayer2++;
        }else if(res.filter(n =>n===this.playername2).length===this.boardsize) {
          this.$toast.warning(this.playername2+"がビンゴ！2点ゲット");
          this.pointPlayer2+=2;
        }
      }
    })
  }

  // ステータスを登録してターン変更
  changeTurn(num: number){
    this.turn? this.status.splice(num,1,this.playername1) : this.status.splice(num,1,this.playername2);
    this.turn = !this.turn;
    this.nextPlayer = this.turn? this.playername1 : this.playername2;
  }

  // ステータス配列を生成
  generateStatusArray(){
    this.status = [];
    for (let i = 0; i < this.boardsize*this.boardsize; i++) {
      this.status.push("");
    }
  }

  // 結果判定配列を生成
  generatePointCheckArray(){
    // 配列初期化
    this.pointCheckArray = [];
    // 縦方向のビンゴ
    for (let i = 0; i < this.boardsize; i++) {
      const checkArrayVertical = [];
      for (let j = 0; j < this.boardsize; j++) {
        checkArrayVertical.push(i+(j*this.boardsize));
      }
      this.pointCheckArray.push(checkArrayVertical);
    }
    // 横方向のビンゴ
    for (let i = 0; i < this.boardsize; i++) {
      const checkArrayHorizontal = [];
      for (let j = 0; j < this.boardsize; j++) {
        checkArrayHorizontal.push((i*this.boardsize)+j);
      }
      this.pointCheckArray.push(checkArrayHorizontal);
    }
    // 斜め方向のビンゴ
    const checkArrayDiagonalRight = []; // 「\」向き
    const checkArrayDiagonalLeft = []; // 「/」向き
    for (let i = 0; i < this.boardsize; i++) {
      checkArrayDiagonalRight.push((this.boardsize+1)*i);
      checkArrayDiagonalLeft.push((this.boardsize-1)*(i+1));
    }
    this.pointCheckArray.push(checkArrayDiagonalRight);
    this.pointCheckArray.push(checkArrayDiagonalLeft);
  }
}
</script>
