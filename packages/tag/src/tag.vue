<template>
  // 
  <transition :name="closeTransition ? '' : 'el-zoom-in-center'">
    <span
      class="el-tag"
      :class="[type ? 'el-tag--' + type : '', {'is-hit': hit}]"
      :style="{backgroundColor: color}">
      <slot></slot>          // 获取el-tag中的非具名路由
      <i class="el-tag__close el-icon-close"   // 渲染出标签后面的关闭标签
        v-if="closable"
        @click="handleClose"></i>            // 监听关闭事件，处理回调。
    </span>
  </transition>
</template>
<script>
  export default {
    name: 'ElTag',               // tag 组件名。
    props: {
      text: String,              // 文案调整。
      closable: Boolean,         // 是否出现关闭按钮
      type: String,              // 标签类型
      hit: Boolean,             // 对应是否有2描边。
      closeTransition: Boolean,   // 关闭动画是否出现。
      color: String              // 背景颜色设置。
    },
    methods: {
      handleClose(event) {        // 点击了关闭图标后， 触发用户自定义的 close 事件。
        this.$emit('close', event);
      }
    }
  };
</script>
