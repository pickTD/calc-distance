<template>
  <div id="app">
    <CalcForm
      v-for="form in forms"
      :key="form"
      :disabled="isFormsDisabled"
      @submitForm="handleSubmitForm"
    />
    <button
      v-if="forms < formsLimit"
      class="button form-add"
      @click="addForm"
    >
      Добавить ещё один калькулятор
    </button>
    <History :history="history"/>
  </div>
</template>

<script>
import moment from 'moment';

import CalcForm from '@/components/CalcForm.vue';
import History from '@/components/History.vue';

const map = window.ymaps;

export default {
  name: 'App',
  components: {
    CalcForm,
    History,
  },
  data() {
    return {
      formsLimit: 10,
      forms: 0,
      history: [],
      isMapsReady: false,
      isFetching: false,
    };
  },
  computed: {
    isFormsDisabled() {
      return !this.isMapsReady || this.isFetching;
    },
  },
  created() {
    this.addForm();
    map.ready(() => this.isMapsReady = true);
  },
  methods: {
    async handleSubmitForm({ from, to }) {
      let result = '';
      let error = false;

      this.isFetching = true;

      try {
        const route = await map.route([from, to]);
        const raw = route.getHumanLength();
        result = this.parseHtmlEntities(raw);
      } catch (e) {
        result = e.message;
        error = true;
      } finally {
        this.isFetching = false;
      }

      const historyElement = {
        value: this.getHistoryString(from, to, result, error),
        error,
      };
      this.history.push(historyElement);
    },
    addForm() {
      this.forms += 1;
    },
    getHistoryString(from, to, result, error) {
      const ts = moment().format('MM/DD HH:mm');
      const route = `${from} -> ${to} = `;
      return `${ts} ${error ? '' : route}${result}`;
    },
    parseHtmlEntities(str) {
      return str.replace(/&#([0-9]{1,3});/gi, (match, numStr) => {
        const num = parseInt(numStr, 10);
        return String.fromCharCode(num);
      });
    },
  },
};
</script>

<style lang="stylus">
*
  box-sizing border-box
#app
  display flex
  flex-direction column
  justify-content center
  align-items center
  max-width 100vw
  margin 4rem auto 0
  color #2c3e50
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  @media(min-width: 768px)
    max-width 1020px
.button
  min-height 2rem
  border-radius .5rem
  outline none
.form-add
  margin 1rem 0
</style>
