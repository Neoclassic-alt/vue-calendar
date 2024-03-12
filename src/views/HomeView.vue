<script setup lang="ts">
import BaseCalendar from '@/components/BaseCalendar.vue'
</script>

<template>
  <main>
    <BaseCalendar
      @click="(date) => console.log(date)"
      :monthNames="[
        'Январь',
        'Февраль',
        'Март',
        'Апрель',
        'Май',
        'Июнь',
        'Июль',
        'Август',
        'Сентябрь',
        'Октябрь',
        'Ноябрь',
        'Декабрь'
      ]"
      date="2015-08-01"
    />
  </main>
</template>
