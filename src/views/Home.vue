<template>
  <div class="home">
    <common-tab
      :items="tabs"
      type="vertical"
      content-position="right"
      :only-icon="true"
      :close-button="true"
      tab-body-style="max-width: 76%; min-width:76%;"
      tab-header-style="border: 1px solid #D7D7D7"
    >
      <div slot="tab1">
      Tab 1
      </div>
      <div slot="tab2">
        Tab 2
      </div>
      <div slot="tab3">
        Tab 3
      </div>
      <div slot="tab4">
        Tab 4
      </div>
      <div slot="tab5">
        Tab 5
      </div>
      <div slot="tab6">
        Tab 6
      </div>
      <div slot="tab7">
        Tab 7
      </div>
    </common-tab>
  </div>
</template>

<script>
import CommonTab from "@/components/CommonTab";
export default {
  "name": "Home",
  "components": {
    CommonTab,
  },
  "data"(){
    return{
      "tabs": [
        {
          "headText": 'Tab 1 head',
          "tooltip": 'Tab 1 head',
          "icon": '@',
          "slot": 'tab1',
          "active":true
        },
        {
          "headText":  'Tab 2 head',
          "tooltip":  'Tab 2 head',
          "icon": '@',
          "slot": 'tab2',
        },
        {
          "headText": 'Tab 3 head',
          "tooltip":  'Tab 3 head',
          "icon": '@',
          "slot": 'tab3',
        },
        {
          "headText": 'Tab 4 head',
          "tooltip": 'Tab 4 head',
          "icon": '@',
          "slot": 'tab4',
        },
        {
          "headText": 'Tab 5 head',
          "tooltip": 'Tab 5 head',
          "icon": '@',
          "slot": 'tab5',
        },
        {
          "headText": 'Tab 6 head',
          "tooltip": 'Tab 6 head',
          "icon": '@',
          "slot": 'tab6',
        },
        {
          "headText": 'Tab 7 head',
          "tooltip": 'Tab 7 head',
          "icon": '@',
          "slot": 'tab7',
        }
      ],

    }
  }
};
</script>
