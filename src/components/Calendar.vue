<template>
  <div class="calendar">
    <div class="calendar-header">
      <div class="left">
        &lt;
      </div>
      <div class="month">
        {{ calendar.month }}
      </div>
      <div class="right">
        &gt;
      </div>
    </div>
    <div class="days-grid">
      <div
        v-for="day in calendar.days"
        :key="day"
        class="day"
        :class="{ active: day === calendar.day }"
      >
        {{ day }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calendar'
}
</script>

<style lang="stylus">
.calendar
  display inline-block
  text-align center

.calendar-header
  align-items center
  display flex
  justify-content space-between

.left,
.right
  font-size: 14px;
  margin 10px

.month
  font-size 16px
  margin-bottom 4px

.days-grid
  display grid
  font-size 14px
  gap 5px
  grid-template-columns repeat(7, 30px)

</style>
