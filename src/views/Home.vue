<template>
  <div class="home">
    <HelloElement></HelloElement>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloElement from "@/components/HelloElement.vue";

export default {
  name: "home",
  components: {
    HelloElement
  }
};
</script>
