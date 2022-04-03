<template>
  <div>
    <section class="score">
      Player <span>{{ this.winCount }}</span> x
      <span>{{ this.loseCount }} </span>
      Computer
    </section>
  </div>
</template>
<script>
export default {
  props: ["winCount", "loseCount"],
};
</script>
<style lang="scss">
section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;

  span {
    margin-right: 5px;
    padding: 8px;
    font-weight: bold;
    border: 1px solid black;
    border-radius: 2px;
  }
}
</style>
