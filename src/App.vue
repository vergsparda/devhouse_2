<template>
  <div id="app">
    <div v-for="item in result" :key="item.ndex">
      {{ item }}
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
      daysList: [
        "monday",
        "tuesday",
        "wednesday",
        "thursday",
        "friday",
        "saturday",
        "sunday"
      ],
      result: null
    };
  },

  mounted() {
    fetch(
      "http://api.dev.cakeiteasy.no/api/store/bakeries/?country_code=no"
    )
      .then(response => response.json())
      .then(json => {
        this.schedule = json[1].schedule;
        this.result = this.scheduleTransform(this.schedule);
      });
  },

  methods: {
    toHours(number) {
      const hours = number / 60;
      const wholeHours = Math.floor(hours);
      const minutes = (hours - wholeHours) * 60;
      const wholeMinutes = Math.round(minutes);
      return `${wholeHours}:${wholeMinutes.toString().padStart(2, "0")}`;
    },

    scheduleTransform(schedules) {
      const daysGroups = [];
      const result = [];
      this.daysList.forEach((day, index) => {
        const schedule = schedules[day];
        const prevDayIndex = index === 0 ? this.daysList.length - 1 : index - 1;
        const prevDayName = this.daysList[prevDayIndex];
        const prevDaySchedule = schedules[prevDayName];

        if (
          prevDaySchedule &&
          this.isScheduleEqual(schedule, prevDaySchedule) &&
          !schedule.day_off
        ) {
          const currentResultIndex = daysGroups.length - 1;
          const currentResult = daysGroups[currentResultIndex];
          currentResult.dayNameEnd = day;
          const {
            dayNameStart,
            dayNameEnd,
            order_before,
            days_before_order
          } = currentResult;

          result[currentResultIndex] = days_before_order
            ? `For ${dayNameStart} - ${dayNameEnd}: before ${order_before}, ${days_before_order} day(s) before`
            : `For ${dayNameStart} - ${dayNameEnd}: before ${order_before}`;
        } else if (schedule.day_off) {
          daysGroups.push(null);
          result.push(`For ${day}: closed`);
        } else {
          const { dayNameStart, order_before, days_before_order } = {
            dayNameStart: day,
            order_before: this.toHours(schedule.order_before),
            days_before_order: schedule.days_before_order
          };

          daysGroups.push({
            dayNameStart,
            order_before,
            days_before_order
          });
          result.push(
            days_before_order
              ? `For ${dayNameStart}: before ${order_before}, ${days_before_order} day(s) before`
              : `For ${dayNameStart}: before ${order_before}`
          );
        }
      });
      return result;
    },

    scheduleToHash({ day_off, days_before_order, order_before }) {
      return [day_off, days_before_order, order_before].toString();
    },

    isScheduleEqual(schedule1, schedule2) {
      const hash1 = this.scheduleToHash(schedule1);
      const hash2 = this.scheduleToHash(schedule2);

      return hash1 === hash2;
    }
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
  text-align: center;
  border: #2c3e50;
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
