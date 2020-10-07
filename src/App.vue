<template>
  <div id="app">
    <Header />
    <Content />
  </div>
</template>

<script>
import "@/assets/scss/style.scss";

import Header from "@/components/Header";
import Content from "@/components/Content";

export default {
  name: "App",
  components: {
    Header,
    Content
  }
};
</script>
