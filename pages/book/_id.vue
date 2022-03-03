<template>
  <div>
    <Form :id=id pagetitle="本の情報詳細" read />
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "nuxt-property-decorator";
@Component
export default class Index extends Vue {
  id: any = "";

  created() {
    console.log("here is _id.vue");
    this.id = this.$route.query.id;
  }
}
</script>