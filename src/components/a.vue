<template>
  <div>
    <input type="text" v-model="text" />
    <button @click="click">B로 전송</button>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import Bus from "@/common/event-bus";

@Component
export default class A extends Vue {
  text = "";
  click(): void {
    Bus.$emit("sendText", this.text);
  }
}
</script>
