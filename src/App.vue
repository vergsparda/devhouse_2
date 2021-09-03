<template>
  <div id="app">
    <div class="box" v-for="item in schedule" :key="item.index">
      <span class="box-item">{{ " For " + item[0] }}</span>
      <span class="box-item">{{ " Day off : " + item[1].day_off }}</span>
      <span class="box-item">{{
        " Days before order : " + item[1].days_before_order
      }}</span>
      <span class="box-item">{{
        " Is relative : " + item[1].is_relative
      }}</span>
      <span class="box-item">{{
        " Order before : " + item[1].order_before
      }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      schedule: [],
      dayStartIndex: 0,
      dayEndIndex: 0,
      days: [
        "mon",
        "tue",
        "wed",
        "thur",
        "fri",
        "sat",
        "sun"
      ],
      scheduleWithoutDays: []
    };
  },

  mounted() {
    fetch(
      "http://api.dev.cakeiteasy.no/api/store/bakeries/test-bakery-pay-in-store/?country=NO"
    )
      .then(response => response.json())
      .then(json => {
        this.schedule = Object.entries(json.schedule);

        console.log(this.schedule);

        this.schedule.forEach(el => {
          this.scheduleWithoutDays.push(el[1]);
        });

        console.log(this.scheduleWithoutDays);
      });
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 850px;
  text-align: left;
}

.box {
  display: flex;
  align-items: left;
  justify-content: space-between;
}

.box-item {
  width: 20%;
}
</style>
