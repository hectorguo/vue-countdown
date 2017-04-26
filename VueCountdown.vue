<template>
    <time>{{text}}</time>
</template>

<script>
import countdown from 'countdown';

// replace setInterval with requestAnimationFrame to improve performance
function setIntervalEx(fn, delay) {

    let start = window.performance.now(),
        handle = {}; // save id in this object for clear this timer

    function loop(current) {
        let delta = current - start;

        if (delta >= delay) {
            fn.call();
            start = window.performance.now();
        }

        handle.id = requestAnimationFrame(loop);
    }

    handle.id = requestAnimationFrame(loop);
    return handle;
}

function clearIntervalEx(handle) {
    cancelAnimationFrame(handle.id);
}


export default {
  name: 'VueCountdown',
  data() {
      return {
          units: countdown.DAYS | countdown.HOURS | countdown.MINUTES,
          years: 0,
          months: 0,
          weeks: 0,
          days: 0,
          hours: 0,
          minutes: 0,
          seconds: 0,
          milliseconds: 0,
          value: 0,
          start: null,
          end: null
      };
  },
  // declare the props
  props: {
      deadline: {
          type: [String, Date],
          default: null
      },
      startDate: {
          type: [String, Date],
          default: null
      },
      // value: {
      //     type: Number,
      //     default: 1000
      // },
      delay: {
          type: Number,
          default: 1000
      },
      max: {
          type: [Number, String],
          default: 0
      },
      format: {
          type: String,
          default: '%Y年 %M月 %D天 %h小时 %m分 %s秒'
      },
      // units: {
      //     type: Number,
      //     default: countdown.DAYS | countdown.HOURS | countdown.MINUTES
      // },
      labelFormat: {
          type: String,
          default: ' 毫秒| 秒| 分| 小时| 天| 周| 个月| 年| 十年| 世纪| 千年'
      },
      // The last separator
      labelLast: {
          type: String,
          default: ' , '
      },
      // separator
      labelDelim: {
          type: String,
          default: ', '
      }
  },
  watch: {
      value(val) {
          if (val <= 0) {
              this.reset();
              this.stop();
              this.$emit('timeover', this.$data); // 代表 时间已到
          }
      },
      text() {
          this.$emit('change', this.$data);
      },
      deadline(val) {
          this.endDateObj = val instanceof Date ? val : new Date(val);
          this.loop();
      },
      startDate(val) {
          this.startDateObj = val instanceof Date ? val : new Date(val);
          this.loop();
      }
  },
  computed: {
      text() {
          return this.formatter();
      }
  },
  methods: {
      reset() {
          this.endDateObj = new Date();
          this.startDateObj = new Date();
          this.refresh();
      },
      loop() {
          this.refresh();
          this.stop();

          this.timer = setIntervalEx(this.refresh, this.delay);
      },
      refresh() {
          const timeSpan = countdown(this.startDateObj, this.endDateObj , this.units, this.max | 0);

          this.start = timeSpan.start;
          this.end = timeSpan.end;
          this.years = timeSpan.years;
          this.months = timeSpan.months;
          this.weeks = timeSpan.weeks;
          this.days = timeSpan.days;
          this.hours = timeSpan.hours;
          this.minutes = timeSpan.minutes;
          this.seconds = timeSpan.seconds;
          this.milliseconds = timeSpan.milliseconds;
          this.value = timeSpan.value;

          this.startDateObj = new Date(this.startDateObj.getTime() + this.delay);
      },
      stop() {
          this.timer && clearIntervalEx(this.timer);
      },
      initUnits() {
          const unitMap = {
                Y: countdown.YEARS,
                M: countdown.MONTHS,
                W: countdown.WEEKS,
                D: countdown.DAYS,
                h: countdown.HOURS,
                m: countdown.MINUTES,
                s: countdown.SECONDS,
                ms: countdown.MILLISECONDS
            };
          let units = 0;

          this.format.replace(/%(Y|M|W|D|h|ms|m|s)([^%]*)/g, (match, key) => {
              units = units | unitMap[key];
          });
          this.units = units;
      },
      formatter() {
          if (!this.format) {
              return;
          }
          const tempMap = {
                  Y: 'years',
                  M: 'months',
                  W: 'weeks',
                  D: 'days',
                  h: 'hours',
                  m: 'minutes',
                  s: 'seconds',
                  ms: 'milliseconds'
              };

          let template;
          let vm = this;

          template = this.format.replace(/%(Y|M|W|D|h|ms|m|s)([^%]*)/g, (match, key, minorKey) => {
              if (!vm[tempMap[key]]) {
                  return '';
              }
              return vm[tempMap[key]] + minorKey;
              // return (vm[tempMap[key]] | 0) + minorKey;
          });
          return template;
      }
  },
  created() {
      // init
      countdown.setFormat({
          singular: this.labelFormat,
          plural: this.labelFormat,
          last: this.labelLast,
          delim: this.labelDelim
      });
      if (!this.deadline) {
          this.endDateObj = new Date();
      } else {
          this.endDateObj = this.deadline instanceof Date ? this.deadline : new Date(this.deadline);
      }
      if(!this.startDate) {
          this.startDateObj = new Date();
      } else {
          this.startDateObj = this.startDate instanceof Date ? this.startDate : new Date(this.startDate);
      }

      this.initUnits();
      this.formatter(); // only for refreshing countdown units
      this.loop();
  }
}
</script>