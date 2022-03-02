<template>
  <p>ここはcomponent/book {{ aaa }}</p>
  
</template>

<script lang="ts">
import { Vue, Component } from "nuxt-property-decorator";
@Component
export default class Book extends Vue {
  aaa = "sasodifu";
}
</script>