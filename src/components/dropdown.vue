<template>
  <div class="dropdown">
    <button
      class="btn btn-secondary dropdown-toggle"
      @click="toggle"
      type="button"
      id="dropdownMenuButton"
      data-toggle="dropdown"
      aria-haspopup="true"
      aria-expanded="false"
    >
      Dropdown button
    </button>
    <div
      class="dropdown-menu show"
      v-show="show"
      aria-labelledby="dropdownMenuButton"
    >
      <a class="dropdown-item" href="#">Action</a>
      <a class="dropdown-item" href="#">Another action</a>
      <a class="dropdown-item" href="#">Something else here</a>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Mixins } from "vue-property-decorator";
import toggle from "./toggle.vue";
@Component({
  components: {
    toggle,
  },
})
export default class Dropdown extends Mixins(toggle) {}
</script>

<style>
.dropdown {
  width: 500px;
}
</style>
