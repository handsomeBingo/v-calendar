<template>
<section id='datepicker' class='section'>
  <h3 class='title has-text-primary is-spaced'>Date Picker</h3>
  <div class='columns is-desktop'>
    <div class='column'>
      <b-tabs>
        <!--DatePicker Overview-->
        <b-tab-item label='Overview'>
          <div class='content'>
            <ul>
              <li>Wraps native calendar to support all native props and events</li>
              <li>3 date selection modes: single date, multiple dates and date range</li>
              <li>Supports disabling of specific dates or date ranges</li>
              <li>Display inline or as a popover for a custom slot</li>
            </ul>
          </div>
        </b-tab-item>
        <!--DatePicker Example Code-->
        <b-tab-item label='Example Code'>
          <code-block :code='exDatePickerCode'></code-block>
        </b-tab-item>
        <!--DatePicker Example Options-->
        <b-tab-item label='Options' icon='gear'>
          <b-field label='Mode'>
            <p class='control-offset'>
              <b-radio v-model='mode' native-value='single'>Single</b-radio>
              <b-radio v-model='mode' native-value='multiple'>Multiple</b-radio>
              <b-radio v-model='mode' native-value='range'>Range</b-radio>
            </p>
          </b-field>
          <b-field grouped>
            <b-field label='Select Color'>
              <b-input v-model='selectColor'></b-input>
            </b-field>
            <b-field label='Drag Color'>
              <b-input v-model='dragColor'></b-input>
            </b-field>
          </b-field>
          <b-field>
            <b-switch v-model='showCaps'>Show Caps</b-switch>
          </b-field>
          <b-field>
            <b-switch v-model='showPopover'>Show Popover</b-switch>
          </b-field>
          <b-field>
            <b-switch v-model='showDisabledDates'>Show Disabled</b-switch>
          </b-field>
          <b-field>
            <b-switch v-model='isInline'>Inline</b-switch>
          </b-field>
          <b-field v-if='isInline'>
            <b-switch v-model='isExpanded'>Expanded</b-switch>
          </b-field>
          <b-field v-else>
            <b-switch v-model='popoverExpanded'>Popover Expanded</b-switch>
          </b-field>
          <div v-if='!isInline'>
            <b-field label='Popover Visibility'>
              <p class='control-offset'>
                <b-radio
                  v-for='v in popoverVisibilities'
                  :key='v'
                  :native-value='v'
                  v-model='popoverVisibility'>
                  {{ v }}
                </b-radio>
              </p>
            </b-field>
            <b-field label='Popover Direction'>
              <p class='control-offset'>
                <b-radio
                  v-for='d in popoverDirections'
                  :key='d'
                  :native-value='d'
                  v-model='popoverDirection'>
                  {{ d }}
                </b-radio>
              </p>
            </b-field>
            <b-field label='Popover Alignment'>
              <p class='control-offset'>
                <b-radio
                  v-for='a in popoverAlignments'
                  :key='a'
                  :native-value='a'
                  v-model='popoverAlignment'>
                  {{ a }}
                </b-radio>
              </p>
            </b-field>
          </div>
        </b-tab-item>
      </b-tabs>
    </div>
    <div class='column'>
      <div :class='{ "example-container": isInline, "popover": !isInline }'>
        <ex-date-picker
          :mode='mode'
          :show-disabled-dates='showDisabledDates'
          :is-inline='isInline'
          :is-expanded='isExpanded'
          :popover-expanded='popoverExpanded'
          :popover-visibility='popoverVisibility'
          :popover-direction='popoverDirection'
          :popover-align='popoverAlignment'
          :select-color='selectColor'
          :drag-color='dragColor'
          :show-caps='showCaps'
          :show-popover='showPopover'>
        </ex-date-picker>
      </div>
    </div>
  </div>
</section>
</template>

<script>
import ExDatePicker from '../examples/ExDatePicker';
import ExDatePickerCode from '!!raw-loader!../examples/ExDatePicker';

export default {
  components: {
    ExDatePicker,
  },
  data() {
    return {
      exDatePickerCode: ExDatePickerCode,
      mode: 'single',
      selectedValue: null,
      showCaps: true,
      showPopover: true,
      showDisabledDates: false,
      isInline: false,
      isExpanded: false,
      popoverExpanded: true,
      popoverVisibility: 'visible',
      popoverVisibilities: ['hover', 'focus', 'visible', 'hidden'],
      popoverDirection: 'bottom',
      popoverDirections: ['bottom', 'top', 'left', 'right'],
      popoverAlignment: 'left',
      selectColor: '#66b3cc',
      dragColor: '#9fcfdf',
    };
  },
  computed: {
    popoverAlignments() {
      switch (this.popoverDirection) {
        case 'bottom':
        case 'top':
          return ['left', 'center', 'right'];
        case 'left':
        case 'right':
          return ['top', 'middle', 'bottom'];
        default:
          return [];
      }
    },
  },
  watch: {
    popoverAlignments(val) {
      if (val && val.length) {
        if (!this.popoverAlignment || !val.includes(this.popoverAlignment)) {
          this.popoverAlignment = val[0];
        }
      }
    },
  },
};
</script>

<style lang='sass' scoped>
  .control-offset
    margin-left: 20px
  .tip
    font-size: 0.8rem
    margin: -14px 0 10px 10px
  .popover
    margin-bottom: 220px
</style>
