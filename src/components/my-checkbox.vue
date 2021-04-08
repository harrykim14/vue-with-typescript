<template>
  <input type="checkbox" :checked="checked" @change="change" />
  <!--:checked="checked"로 모델의 변수명을 명시함으로써 이 컴포넌트를 사용하는 객체로 값이 넘어감 -->
</template>

<script lang="ts">
import { Vue, Component, Model, Emit } from "vue-property-decorator";

@Component
export default class MyChecbox extends Vue {
  @Model("change", { type: Boolean }) readonly checked!: boolean;

  @Emit()
  change(event: Event): boolean {
    const target = event.target as HTMLInputElement;
    return target.checked;
  }
}
</script>

<style scoped></style>
