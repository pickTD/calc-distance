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

<style scoped lang="stylus">
.history
  &__container
    width 100%
    margin 0
  &__list
    margin 0
    padding 0
    list-style none
  &__element
    font-size .9rem
    margin-bottom .5rem
.error
  color red
</style>
