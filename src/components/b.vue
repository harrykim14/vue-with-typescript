<template>
  <div>A에서 작성한 메세지는? => {{ text }}</div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import Bus from "@/common/event-bus";

@Component
export default class A extends Vue {
  text = "";
  created(): void {
    Bus.$on("sendText", (text: string) => {
      this.text = text;
    });
  }
}
</script>

<style scoped></style>
