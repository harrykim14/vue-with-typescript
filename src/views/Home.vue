<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <children @counter="counter"></children>
    <p>부모에서 숫자를 보여줍니다. : {{ count }}</p>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import children from "@/components/children.vue";

@Component({
  components: {
    children,
  },
})
export default class Home extends Vue {
  count = 0;

  counter() {
    this.count++;
  }
}
</script>
