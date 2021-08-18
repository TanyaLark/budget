<template>
  <div class="total-value" :class="setTextColor">
    Balance: {{ total }}
  </div>
</template>

<script>
export default {
  name: "TotalBalance",
  props: {
    total: {
      type: Number,
      default: 0
    }
  },
  computed: {
    setTextColor(){
      let color;

      switch (true) {
        case this.total > 0:
          color = "green";
          break;
        case this.total === 0:
          color = "black";
          break;
        case this.total < 0:
          color = "red";
          break;
      }

      return color;
    }
  }

}
</script>

<style scoped>
  .total-value{
    font-size: 26px;
    text-transform: uppercase;
    padding: 20px;
    text-align: center;
  }

  .green{
    color: green;
  }

  .black{
    color: black;
  }

  .red{
    color: red;
  }
</style>

