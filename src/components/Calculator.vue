<template>
  <div class="calculator">
    <div class="display"><p>0.60000000018</p></div>
    <div class="clear nan">AC</div>
    <div class="backspace nan"><b-icon-backspace></b-icon-backspace></div>
    <div class="percentage nan">%</div>
    <div class="nan">/</div>
    <div>7</div>
    <div>8</div>
    <div>9</div>
    <div class="nan">*</div>
    <div>4</div>
    <div>5</div>
    <div>6</div>
    <div class="nan">-</div>
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div class="nan">+</div>
    <div class="transform"><b-icon-arrow-repeat></b-icon-arrow-repeat></div>
    <div>0</div>
    <div class="dot">.</div>
    <div class="nan">=</div>
  </div>
</template>

<script>
export default {

}
</script>


<style scoped>
  .calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(80px, auto);
    width: 420px;
    margin: 0 auto;
    background-color: #eee;
    font-size: 2rem;
    border-radius: 5px;
  }

  .display {
    grid-column: 1 / 5;
    grid-row: 1 / 3;
    background-color: #aaa;
    border-radius: 5px 5px 0 0;
    margin-bottom: 1rem;
    position: relative;
  }
  .display p {
    position: absolute;
    bottom: 0;
    right: 0;
    text-align: center;
  }

  .nan {
    /* background-color: orange; */
    color: orange;
  }

  .transform {
    cursor: pointer;
  }


</style>
