<script>
import CalculatorBody from "@/components/CalculatorBody.vue";

export default {
  name: "App",
  components: {
    CalculatorBody
  }
}
</script>

<template>
  <main>
    <calculator-body></calculator-body>
  </main>
</template>

<style lang="scss">
main {
  display: flex;
  min-height: 100vh;
  background: #1c222a;
  transition: color 0.5s, background-color 0.5s;
  line-height: 1.6;
  font-family: "JetBrains Mono Light", Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu,
  Cantarell, 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;;
  font-size: 26px;
}
</style>
