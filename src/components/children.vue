<template>
  <div>
    <button @click="counter">자식에서 숫자를 증가시킵니다.</button>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Emit } from "vue-property-decorator";

@Component
export default class Children extends Vue {
  @Emit()
  counter() {
    console.log("count");
  }
}
</script>
