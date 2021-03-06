<style lang="scss">
  .datepicker-container {
    position: relative;
  }
</style>

<template>
  <div class="datepicker-container">
    <input type="text"
           :class="classDesign"
           :value="date_formatted"
           @click="showDatepicker">

    <input type="hidden"
           :id="id"
           :name="name"
           :value="date_raw">

    <datepicker-agenda :cellWidth="cellWidth"
                       :disable-passed-days="disablePassedDays"
                       :doubled="doubled"
                       :disabled-days="disabledDays"
                       :available-periods="availablePeriods"
                       :lang="lang"
                       :orientation="orientation"
                       :show="isVisible"
                       :header="header"
                       @change="selectDate"
                       @hide="hideDatePicker"
                       @cancel="cancelDateSelection">
    </datepicker-agenda>
  </div>
</template>

<script>
  import moment from 'moment';

  import DatepickerAgenda from './components/DatepickerAgenda.vue';

  export default {
    model: {
      prop: 'date_formatted',
      event: 'change'
    },
    components: {
      'datepicker-agenda': DatepickerAgenda
    },
    props: {
      cellWidth: { type: [Number, String] },
      header: { type: Boolean, default: true },
      classDesign: { type: String, default: '' },
      disablePassedDays: { type: Boolean, default: false },
      availablePeriods: {
        type: Array, default() {
          return []
        }
      },
      disabledDays: {
        type: Array, default() {
          return []
        }
      },
      doubled: { type: Boolean, default: false },
      format: { type: String, default: 'YYYY-MM-DD' },
      id: { type: String, default: 'vue-datepicker' },
      lang: { type: String, default: 'en' },
      name: { type: String, default: 'datepicker' },
      orientation: { type: String, default: 'portrait' }
    },
    data() {
      return {
        date: '',
        isVisible: false
      };
    },
    computed: {
      date_formatted() {
        if (this.date) return this.date.format(this.format);
        return '';
      },
      date_raw() {
        if (this.date) return this.date.format('YYYY-MM-DD');
        return '';
      }
    },
    mounted() {
      moment.locale(this.lang);
    },
    methods: {
      selectDate(newDate) {
        this.date = newDate;
        this.$emit('change', this.date_formatted);
      },
      showDatepicker() {
        this.isVisible = true;
        setTimeout(() => document.addEventListener('click', this.hideDatePicker), 0);
      },
      hideDatePicker() {
        this.isVisible = false;
        document.removeEventListener('click', this.hideDatePicker);
      },
      cancelDateSelection() {
        this.hideDatePicker();
      }
    }
  };
</script>