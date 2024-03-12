<script setup lang="ts">
import { ref, computed } from 'vue'

interface Props {
  monthNames?: string[]
  weekdayNames?: string[]
  date?: string | Date
  onClick?: (date: Date) => void
}

const props = withDefaults(defineProps<Props>(), {
  monthNames: () => [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'Jule',
    'August',
    'September',
    'October',
    'November',
    'December'
  ],
  weekdayNames: () => ['Пон', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'],
  date: () => new Date()
})

function daysInMonth(month, year) {
  return new Date(year, month, 0).getDate()
}

let viewDate = ref(new Date(props.date))

let selectedDate = ref(new Date(props.date))

const firstWeekdayInMonth = computed(() => {
  const _ = new Date(viewDate.value.getTime())
  _.setDate(1)
  return _.getDay() // 0 - воскресенье в JavaScript
})

const dayInMonth = computed(() =>
  daysInMonth(viewDate.value.getMonth() + 1, viewDate.value.getFullYear())
)

function prevMonth() {
  viewDate.value.setMonth(viewDate.value.getMonth() - 1)
  viewDate.value = new Date(viewDate.value)
}

function nextMonth() {
  viewDate.value.setMonth(viewDate.value.getMonth() + 1)
  viewDate.value = new Date(viewDate.value)
}

const showHiddenLine = computed(() => {
  return !(
    (firstWeekdayInMonth.value === 0 && dayInMonth.value >= 30) ||
    (firstWeekdayInMonth.value === 6 && dayInMonth.value == 31)
  )
})
</script>

<template>
  <div class="calendar">
    <div class="calendar__header">
      <p @click="prevMonth" class="change-month">◀</p>
      <p>{{ monthNames[viewDate.getMonth()] }} {{ viewDate.getFullYear() }}</p>
      <p @click="nextMonth" class="change-month">▶</p>
    </div>
    <div class="calendar__weekdays">
      <span v-for="weekday in props.weekdayNames" :key="weekday" class="calendar__weekday">
        {{ weekday }}</span
      >
    </div>
    <div class="calendar__days">
      <!-- наиболее простой способ обеспечить расположение дат - flex + flex-wrap -->
      <span
        :style="{
          'margin-left': `calc(100% / 7 * ${firstWeekdayInMonth ? firstWeekdayInMonth - 1 : 6})`
        }"
      ></span>
      <span
        v-for="day in dayInMonth"
        :key="day"
        class="calendar__day"
        :class="{
          'calendar__selected-day':
            selectedDate.getDate() == day &&
            selectedDate.getMonth() == viewDate.getMonth() &&
            selectedDate.getFullYear() == viewDate.getFullYear()
        }"
        @click="(selectedDate = new Date(viewDate.setDate(day))), onClick?.(selectedDate)"
        >{{ day }}</span
      >
      <span v-show="showHiddenLine" class="calendar__day_hidden-line">0</span>
    </div>
  </div>
</template>

<style scoped>
.calendar {
  max-width: 260px;
  border: 1px solid gainsboro;
  padding: 10px;
}
.calendar__header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 5px 10px;
}
.calendar__weekdays {
  font-size: 0.8em;
  display: flex;
  justify-content: space-between;
}
.calendar__weekday {
  width: calc(100% / 7);
  text-align: center;
}
.calendar__days {
  display: flex;
  flex-wrap: wrap;
}
.calendar__day,
.calendar__day_hidden-line {
  padding: 5px 0;
  width: calc(100% / 7);
  text-align: center;
  cursor: pointer;
}
.calendar__day_hidden-line {
  visibility: hidden;
  width: 100%;
}
.change-month {
  cursor: pointer;
  user-select: none;
}
.calendar__selected-day {
  outline: 1px solid brown;
}
</style>
