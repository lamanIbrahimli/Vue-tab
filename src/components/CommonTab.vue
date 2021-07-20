<template>
  <div class="tab"
       :class="[
             {'tab--vertical': type === 'vertical', 'tab--icon': onlyIcon},
             contentPosition ? 'tab--vertical--' + contentPosition : '',
             tabClass
         ]"
       :style="tabStyle"
  >
    <div class="tab__header" :class="tabHeaderClass" :style="tabHeaderStyle">
      <a @click="tabChange($event)"
         :key="index"
         :data-id="item.slot"
         class="tab__link"
         :class="[{'tab__link--active': item.active, 'tab__link--tooltip': item.tooltip}, tabLinkClass]"
         :style="tabLinkStyle"
         v-for="(item, index) in items"
         :data-tooltip="item.tooltip"
      >
        <i v-if="item.icon">{{item.icon}}</i>
        <!-- istenilen iconu inistall edib (:class="['icon ' + item.icon]") bu cod ile deyise bilersiz       -->
        <span v-if="item.title && !onlyIcon" :class="{'ml-1': item.icon}">{{ item.title }}</span>
      </a>
    </div>
    <div class="tab__body" :class="[tabBodyClass, {'d-none': close}]" :style="tabBodyStyle">
      <div class="tab__item"
           :class="[
                     {'tab__item--active': item.active},
                     tabItemClass
                 ]"
           :style="tabItemStyle"
           :key="index"
           :id="item.slot"
           v-for="(item, index) in items"
      >
        <div class="tab__item__head" v-if="item.headText">
          <div class="tab__item__head__title">{{ item.headText}}</div>
          <a @click="tabClose" class="close-icon">
            <i class="icon icon-close-circle"></i>
          </a>
        </div>
        <div class="tab__item__content">
          <slot :name="item.slot"></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CommonTab",
  props: {
    onlyIcon: {
      default: false
    },
    closeButton: {
      default: false
    },
    type: {
      default: null
    },
    contentPosition: {
      default: null
    },
    tabClass: {
      default: null
    },
    tabStyle: {
      default: null
    },
    tabHeaderClass: {
      default: null
    },
    tabHeaderStyle: {
      default: null
    },
    tabBodyClass: {
      default: null
    },
    tabBodyStyle: {
      default: null
    },
    tabLinkClass: {
      default: null
    },
    tabLinkStyle: {
      default: null
    },
    tabItemClass: {
      default: null
    },
    tabItemStyle: {
      default: null
    },
    items: {
      default: Array,
      type: Array,
      required: true
    },
  },
  data() {
    return {
      close: false
    }
  },
  created() {
    this.close = this.closeButton;
  },
  methods: {
    tabChange($event) {
      const obj = $event.target.closest('a') ? $event.target.closest('a') : $event.target;
      const id = obj.getAttribute('data-id');
      Array.from(document.querySelectorAll('.tab__link')).forEach(i => {
        i.classList.remove('tab__link--active')
      })
      document.querySelectorAll('[data-id=' + id + ']')[0].classList.add('tab__link--active');
      Array.from(document.querySelectorAll('.tab__item')).forEach(i => {
        i.classList.remove('tab__item--active');
      })
      document.querySelectorAll('#' + id)[0].classList.add('tab__item--active');
      document.querySelectorAll('.tab__body')[0].classList.remove('d-none');
    },
    tabClose() {
      Array.from(document.querySelectorAll('.tab__item')).forEach(i => {
        i.classList.remove('tab__item--active');
      })
      Array.from(document.querySelectorAll('.tab__link')).forEach(i => {
        i.classList.remove('tab__link--active')
      })
      document.querySelectorAll('.tab__body')[0].classList.add('d-none')
    }
  }
}
</script>

<style scoped lang="scss">
.tab {
  display: flex;
  flex-direction: column;
  &__header {
    display: flex;
    background: #fff;
    box-shadow: 0px 10px 28px rgba(0, 0, 0, 0.11) !important;
    width: 100%;
    border-radius: 5px;
    padding: 11px;
  }
  &__link {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 0 20px;
    height: 36px;
    color: #8D8D8D !important;
    border-radius: 5px;
    position: relative;
    &:not(:last-child) {
      margin-right: 10px;
    }
    &:hover {
      background: rgba(0, 0, 0, 0.08);
    }
    .icon {
      font-size: 16px;
      position: relative;
      top: -1px;
    }
    &--active {
      background: #2D6E90 !important;
      color: #fff !important;
      font-weight: 500;
    }
    &--tooltip {
      &:after {
        content: attr(data-tooltip);
        position: absolute;
        top: calc(100% - 10px);
        width: fit-content;
        left: 0;
        right: 0;
        z-index: 999;
        margin: auto;
        padding: 0 15px;
        white-space: nowrap;
        background: #8D8D8D;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 36px;
        color: #fff !important;
        border-radius: 4px;
        box-shadow: 0px 5px 21px rgba(0, 0, 0, 0.1) !important;
        opacity: 0;
        visibility: hidden;
      }
      &:before {
        content: '';
        position: absolute;
        top: calc(100% - 20px);
        left: 0;
        right: 0;
        margin: auto;
        z-index: 999;
        width: 10px;
        height: 10px;
        border-width: 10px;
        border-style: solid;
        border-color: transparent transparent #8d8d8d transparent;
        opacity: 0;
        visibility: hidden;
      }

      &:hover:before, &:hover:after {
        opacity: 1;
        visibility: visible;
      }
      &:hover:before {
        top: calc(100% - 10px);
      }
      &:hover:after {
        top: calc(100% + 10px);
      }
    }
  }
  &__body {
    display: flex;
    flex-direction: column;
    height: 100%;
    background: #fff;
    margin-top: 10px;
    padding: 25px;
    box-shadow: 0px 10px 28px rgba(0, 0, 0, 0.11) !important;
    border-radius: 5px;
  }
  &__item {
    &:not(.tab__item--active) {
      display: none;
    }
    &--active {
      display: block !important;
    }
    display: flex;
    flex-direction: column;
    &__head {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 18px;
      &__title {
        font-size: 18px;
        font-weight: 500;
      }
      .close-icon {
        font-size: 20px;
        color: #B3B3B3 !important;
      }
    }
    &__content {
      height: 100%;
      flex: 1;
    }
  }

  &--vertical {
    flex-direction: row;
    align-items: flex-start;

    .tab__header {
      width: auto;
      flex-direction: column;
    }

    .tab__link {
      margin-left: 0;
      width: 100%;
      margin-bottom: 5px;
    }

    .tab__body {
      flex: 1;
      margin-top: 0;
    }

    &--left {
      flex-direction: row-reverse;
      .tab__body {
        margin-right: 10px;
      }
      .tab__link--tooltip {
        &:after {
          top: 0;
          bottom: 0;
          right: calc(100% + 20px);
          left: initial;
        }
        &:before {
          top: 0;
          bottom: 0;
          right: 0;
          border-color: transparent transparent transparent #8d8d8d;
          left: calc(-100% - 20px);
        }
      }
    }
    &--right {
      .tab__body {
        margin-left: 10px;
      }
      .tab__link--tooltip {
        &:after {
          top: 0;
          bottom: 0;
          left: calc(100% + 20px) !important;
        }
        &:before {
          top: 0;
          bottom: 0;
          border-color: transparent #8d8d8d transparent transparent;
          left: 100%;
        }
      }
    }
  }

  &--icon {
    .tab__link {
      padding: 0;
      width: 44px;
      height: 44px;
      border-radius: 10px;
      margin-right: 0;
    }
  }
}

</style>
