<template>
<v-app light>
  <v-container>
    <!-- PC版ヘッダー -->
    <v-toolbar
      color="secondary"
      class="d-none d-sm-block"
    >
      <v-tabs>
        <v-tab to="/game">
          <v-img
            src=favicon.png
            max-height="50"
            max-width="50"
          ></v-img>
        </v-tab>
        <v-tab to="/">カード</v-tab>
        <v-tab to="/bookList">一覧</v-tab>
        <v-tab to="/bookInput">登録</v-tab>
      </v-tabs>
      <v-card-text><h4>あなたの好きな本を教えてください</h4></v-card-text>
    </v-toolbar>
    <!-- SP版ヘッダー -->
    <v-app-bar
      color="secondary"
      class="d-block d-sm-none"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <nuxt-link to="/game">
        <v-img
          src=favicon.png
          max-height="50"
          max-width="50"
        ></v-img>
      </nuxt-link>
      <v-card-text><b>あなたの好きな本を教えてください</b></v-card-text>
    </v-app-bar>
    <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
      color="accent"
    >
      <v-list nav dense>
        <v-list-item-group
          v-model="group"
        >
          <v-list-item to="/">
            <v-list-item-title>カード</v-list-item-title>
          </v-list-item>
          <v-list-item to="/bookList">
            <v-list-item-title>一覧</v-list-item-title>
          </v-list-item>
          <v-list-item to="/bookInput">
            <v-list-item-title>登録</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
    <br>
    <Nuxt />
  </v-container>
</v-app>
</template>

<script land="ts">
import {Component, Vue, Watch} from "nuxt-property-decorator";
@Component
export default class Default extends Vue {
  drawer = false;
  group = null;

  @Watch
  group(){
    this.drawer = false;
  }

}
</script>
