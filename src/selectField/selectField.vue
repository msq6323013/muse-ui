<template>
<text-field ref="textField" class="mu-select-field" :underlineShow="underlineShow"
  :labelFloat="labelFloat" :hintText="hintText" :fullWidth="fullWidth"
  :helpText="helpText" :icon="icon" :value="inputValue instanceof Array ? inputValue.join('') : inputValue"
   :disabled="disabled" :errorText="errorText" :errorColor="errorColor" :label="label">
  <dropDown-menu :anchorEl="anchorEl" :scroller="scroller" @open="handleOpen" @close="handleClose"  @change="handlehange" :value="inputValue" :disabled="disabled" :maxHeight="maxHeight" :autoWidth="autoWidth"
    :multiple="multiple" :anchorOrigin="{vertical: 'bottom', horizontal: 'left'}">
    <slot></slot>
  </dropDown-menu>
</text-field>
</template>

<script>
import textField from '../textField'
import dropDownMenu from '../dropDownMenu'
export default {
  name: 'mu-select-field',
  props: {
    label: {
      type: String
    },
    labelFloat: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    hintText: {
      type: String
    },
    helpText: {
      type: String
    },
    errorText: {
      type: String
    },
    errorColor: {
      type: String
    },
    icon: {
      type: String
    },
    maxHeight: {
      type: Number
    },
    autoWidth: {
      type: Boolean,
      default: false
    },
    fullWidth: {
      type: Boolean,
      default: false
    },
    underlineShow: {
      type: Boolean,
      default: true
    },
    value: {},
    multiple: {
      type: Boolean,
      default: false
    },
    scroller: {
      type: [window.HTMLDocument, window.Element, window.Window]
    }
  },
  data () {
    let defaultVal = this.value
    if (!defaultVal) defaultVal = ''
    if (this.multiple && !(defaultVal instanceof Array)) {
      defaultVal = []
    }
    return {
      anchorEl: null,
      inputValue: defaultVal
    }
  },
  mounted () {
    this.anchorEl = this.$children[0].$refs.input
  },
  methods: {
    handlehange (val) {
      if (val === this.inputValue) return
      if (this.multiple) {
        const index = this.inputValue.indexOf(val)
        if (index === -1) {
          this.inputValue.push(val)
        } else {
          this.inputValue.splice(index, 1)
        }
      } else {
        this.inputValue = val
      }
      this.$emit('change', this.inputValue)
    },
    handleOpen () {
      this.$refs.textField.handleFocus()
    },
    handleClose () {
      this.$refs.textField.handleBlur()
    }
  },
  watch: {
    value (val) {
      this.inputValue = val
    },
    inputValue (val, oldVal) {
      if (val === oldVal) return
      this.$emit('input', val)
    }
  },
  components: {
    'text-field': textField,
    'dropDown-menu': dropDownMenu
  }
}
</script>

<style lang="less">
@import "../styles/import.less";
.mu-select-field{
  .mu-dropDown-menu{
    display: block;
    width: 100%;
    height: 32px;
  }
  .mu-dropDown-menu-text{
    line-height: 32px;
    height: 32px;
    padding-left: 0px;
    padding-right: 24px;
    word-wrap: break-word;
    overflow: hidden;
  }
  .mu-dropDown-menu-line{
    display: none;
  }
  .mu-dropDown-menu-icon{
    right: 0;
    top: 6px;
  }
}
</style>
