<template>
  <form
    class="calc-form"
    @submit.prevent="submitForm"
  >
    <input type="text" class="calc-form__input" v-model="from">
    <input type="text" class="calc-form__input" v-model="to">
    <div class="calc-form__buttons">
      <button
        type="submit"
        class="button calc-form__button"
        :disabled="isSubmitDisabled"
      >
        Рассчитать
      </button>
      <button
        class="button calc-form__button"
        :disabled="isClearDisabled"
        @click.prevent="clearForm"
      >
        Очистить
      </button>
    </div>
  </form>
</template>

<script>
export default {
  name: 'CalcForm',
  props: {
    disabled: Boolean,
  },
  data() {
    return {
      from: '',
      to: '',
    };
  },
  computed: {
    isSubmitDisabled() {
      return this.disabled || !(this.from && this.to);
    },
    isClearDisabled() {
      return this.disabled || (!this.from && !this.to);
    },
  },
  methods: {
    clearForm() {
      this.from = '';
      this.to = '';
    },
    submitForm() {
      this.$emit('submitForm', { from: this.from.trim(), to: this.to.trim() });
    },
  },
  watch: {
    from(newVal) {
      if (newVal && newVal.length === 1) {
        this.from = newVal.toUpperCase();
      } else if (newVal) {
        this.from = newVal.charAt(0).toUpperCase() + newVal.slice(1);
      }
    },
    to(newVal) {
      if (newVal && newVal.length === 1) {
        this.to = newVal.toUpperCase();
      } else if (newVal) {
        this.to = newVal.charAt(0).toUpperCase() + newVal.slice(1);
      }
    },
  },
};
</script>

<style scoped lang="stylus">
.calc-form
  display flex
  flex-direction column
  justify-content center
  align-items center
  width 100%
  margin-bottom 1rem
  @media(min-width: 768px)
    flex-direction row
  &__input
    width 100%
    margin-bottom .5rem
    padding .5rem
    border-radius .5rem
    border 1px solid grey
    outline none
    @media(min-width: 768px)
      margin-bottom 0
      margin-right .5rem
  &__buttons
    display flex
    justify-content space-between
  &__button
    margin-right .5rem
    &:last-child
      margin 0
</style>
