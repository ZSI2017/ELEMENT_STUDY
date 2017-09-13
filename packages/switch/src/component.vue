<template>
  <label class="el-switch" :class="{ 'is-disabled': disabled, 'el-switch--wide': hasText, 'is-checked': checked }">
    <div class="el-switch__mask" v-show="disabled"></div>


    <!-- 使用checkbox 里面的 change事件 来监听点击事件的改变 -->
    <input
      class="el-switch__input"
      type="checkbox"
      @change="handleChange"
      ref="input"
      :name="name"
      :true-value="onValue"
      :false-value="offValue"
      :disabled="disabled">
      <!--     -->
    <span class="el-switch__core" ref="core" :style="{ 'width': coreWidth + 'px' }">
      <span class="el-switch__button" :style="{ transform }"></span>
    </span>
    <transition name="label-fade">
      <div
        class="el-switch__label el-switch__label--left"
        v-show="checked"
        :style="{ 'width': coreWidth + 'px' }">
        <i :class="[onIconClass]" v-if="onIconClass"></i>
        <span v-if="!onIconClass && onText">{{ onText }}</span>
      </div>
    </transition>
    <transition name="label-fade">
      <div
        class="el-switch__label el-switch__label--right"
        v-show="!checked"
        :style="{ 'width': coreWidth + 'px' }">
        <i :class="[offIconClass]" v-if="offIconClass"></i>
        <span v-if="!offIconClass && offText">{{ offText }}</span>
      </div>
    </transition>
  </label>
</template>

<script>
  export default {
    name: 'ElSwitch',
    props: {
      value: {
        type: [Boolean, String, Number],
        default: false
      },
      disabled: {
        type: Boolean,
        default: false
      },
      //  设置switch 的宽度，（单位像素） 默认值
      width: {
        type: Number,
        default: 0
      },
   // 打开时图标的类名。
      onIconClass: {
        type: String,
        default: ''
      },
  // switch 关闭时 显示图标的类名。
      offIconClass: {
        type: String,
        default: ''
      },
// switch打开时文字
      onText: {
        type: String,
        default: 'ON'
      },
// switch 关闭时文字。
      offText: {
        type: String,
        default: 'OFF'
      },
// switch 打开时的背景色
      onColor: {
        type: String,
        default: ''
      },
// switch 关闭时的背景色。
      offColor: {
        type: String,
        default: ''
      },
// 打开时的值
      onValue: {
        type: [Boolean, String, Number],
        default: true
      },
// 关闭时的值。
      offValue: {
        type: [Boolean, String, Number],
        default: false
      },
// switch 对应的name 的值。
      name: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
      //初始化单元格的宽度
        coreWidth: this.width
      };
    },
    created() {
      if (!~[this.onValue, this.offValue].indexOf(this.value)) {
        this.$emit('input', this.offValue);
      }
    },
    computed: {
      checked() {
          //当前的value 值，如果是打开时的value 值，就表示是checked；
        return this.value === this.onValue;
      },
      hasText() {
        // 判断当前是否已经设置了文本。
        /* istanbul ignore next */
        return this.onText || this.offText;
      },
      transform() {
        return this.checked ? `translate(${ this.coreWidth - 20 }px, 2px)` : 'translate(2px, 2px)';
      }
    },
    watch: {
      checked() {
    // 监听checked 事件发生变化
   // 监听到任何变化就触发改变背景颜色
        if (this.onColor || this.offColor) {
          this.setBackgroundColor();
        }
      }
    },
    methods: {
      handleChange(event) {
        this.$emit('change', !this.checked ? this.onValue : this.offValue);
        this.$emit('input', !this.checked ? this.onValue : this.offValue);
        this.$nextTick(() => {
          // set input's checked property
          // in case parent refuses to change component's value
          this.$refs.input.checked = this.checked;
        });
      },
      setBackgroundColor() {
        let newColor = this.checked ? this.onColor : this.offColor;
        this.$refs.core.style.borderColor = newColor;
        this.$refs.core.style.backgroundColor = newColor;
      }
    },
    mounted() {
      /* istanbul ignore if */
      if (this.width === 0) {
        this.coreWidth = this.hasText ? 58 : 46;
      }
      if (this.onColor || this.offColor) {
        this.setBackgroundColor();
      }
      this.$refs.input.checked = this.checked;
    }
  };
</script>
