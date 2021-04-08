<template>
  <div>
    {{ message }}
  </div>
</template>

<script lang="ts">
import { Component, Vue, Inject } from "vue-property-decorator";

@Component
export default class Children extends Vue {
  @Inject() readonly message!: string; // !: 값이 초기화 되지 않은 것이 의도적인것임을 나타냄
}
</script>
