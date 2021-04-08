<template>
  <div>
    <A />
    <B />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import A from "@/components/a.vue";
import B from "@/components/b.vue";
@Component({
  components: {
    A,
    B,
  },
})
export default class App extends Vue {}
</script>

<style></style>
