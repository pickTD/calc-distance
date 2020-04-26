<template>
  <div class="history__container">
    <h2 class="history__title">Журнал запросов:</h2>
    <ul
      v-if="history.length"
      class="history__list"
    >
      <li
        v-for="(record, idx) in displayedHistory"
        :key="idx"
        class="history__element"
        :class="{error: record.error}"
      >
        {{ record.value }}
      </li>
    </ul>
    <p v-else>Пока нет ни одного результата</p>
  </div>
</template>

<script>
export default {
  name: 'History',
  props: {
    history: { type: Array, required: true },
  },
  computed: {
    displayedHistory() {
      return this.history.slice().reverse();
    },
  },
};
</script>
