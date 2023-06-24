<template lang="">
    <button 
        class="el-button"
        @click="handleClick"
        :disabled="buttonDisabled || loading"
        :autofocus="autofocus"
        :type="nativeType"
        :class="[
            type ? 'el-button--' + type : '',
            buttonSize ? 'el-button--' + buttonSize : '',
            // 当值为真时绑定键给class，例如buttonDisabled为真，则给class绑定‘is-disabled’
            {
                'is-disabled': buttonDisabled,
                'is-loading': loading,
                'is-plain': plain,
                'is-round': round,
                'is-circle': circle
            }
        ]"
    >
    <!-- 使用v-if判断，展示loading图标或者是其他图标 -->
    <i class="el-icon-loading" v-if="loading"></i>
    <i :class="icon" v-if="icon && !loading"></i>
    <!-- $slots.default获取非具名插槽的内容，如果存在就放在slot中 -->
    <span v-if="$slots.default"><slot></slot></span>
    </button>
</template>
<script>
export default {
  name: "ElButton",

    // 可能会有祖先组件控制此按钮，inject来获取祖先传递的数据
  inject: {
      elForm: {
        default: ''
      },
      elFormItem: {
        default: ''
      }
    },

    // 基本上为控制按钮样式的值
  props: {
    // 按钮的类型
    type: {
      type: String,
      default: "default",
    },
    // 按钮的大小
    size: String,
    // 按钮中的图标
    icon: {
      type: String,
      default: "",
    },
    // 按钮的type
    nativeType: {
      type: String,
      default: "button",
    },
    // 是否加载中
    loading: Boolean,
    // 是否可点击
    disabled: Boolean,
    // 空心？
    plain: Boolean,
    // 默认选中（自动对焦？）
    autofocus: Boolean,
    // 圆角
    round: Boolean,
    // 圆按钮
    circle: Boolean,
  },

  computed: {
    // 如果按钮在表单中，则返回表单项元素中控制子组件的尺寸大小（elFormItemSize），否则返回undefined
    _elFormItemSize() {
      return (this.elFormItem || {}).elFormItemSize;
    },
    // 根据顺序来决定button的大小
    buttonSize() {
      return this.size || this._elFormItemSize || (this.$ELEMENT || {}).size;
    },
    // 如果props中存在disabled，那么选用它的值，否则使用this.elForm.disabled或者undefined
    buttonDisabled() {
      return this.$options.propsData.hasOwnProperty("disabled")
        ? this.disabled
        : (this.elForm || {}).disabled;
    },
  },

  methods: {
    // 处理点击事件，通过this.$emit("click", evt)，向上传递click事件，并且将事件源evt传递
    handleClick(evt) {
      this.$emit("click", evt);
    },
  },
};
</script>
<style lang="">
</style>