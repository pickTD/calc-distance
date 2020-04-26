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
        :disabled="isButtonsDisabled"
      >
        Рассчитать
      </button>
      <button
        class="button calc-form__button"
        :disabled="isButtonsDisabled"
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
    isButtonsDisabled() {
      return this.disabled || !(this.from && this.to);
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
