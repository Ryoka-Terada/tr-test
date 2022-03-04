<template>
<v-app light>
  <v-container>
    <Header />
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
