<template>
  <div class="mdl-textfield mdl-js-textfield"
       :class="{'mdl-textfield--floating-label': floatingLabel, 'mdl-textfield--expandable': expandable, 'is-dirty': isDirty, 'is-disabled': disabled}">
    <slot v-if="expandable" name="expandable-button">
      <label class="mdl-button mdl-js-button mdl-button--icon" :for="id">
        <i class="material-icons">{{expandable}}</i>
      </label>
    </slot>
    <div :class="{'mdl-textfield__expandable-holder': expandable}">
      <slot v-if="textarea" name="textarea">
        <textarea class="mdl-textfield__input"
                  ref="focusTarget"
                  type="text"
                  :value="value"
                  @input="fireInputEvent"
                  @change="fireChangeEvent"
                  @blur="fireBlurEvent"
                  @focus="fireFocusEvent"
                  :required="required"
                  :id="id"
                  :rows="rows"
                  :maxlength="maxlength">
        </textarea>
      </slot>
      <slot v-else="v-else" name="input">
        <input class="mdl-textfield__input"
               ref="focusTarget"
               :type="type"
               :value="value"
               @input="fireInputEvent"
               @change="fireChangeEvent"
               @blur="fireBlurEvent"
               @focus="fireFocusEvent"
               :id="id" :pattern="pattern"
               :disabled="disabled"
               :required="required"
               :readonly="readonly"
               :maxlength="maxlength"/>
      </slot>
      <slot name="label">
        <label class="mdl-textfield__label" :for="id">{{displayLabel}}</label>
      </slot>
      <slot name="error">
        <label class="mdl-textfield__error" v-if="error">{{error}}</label>
      </slot>
    </div>
  </div>
</template>

<script>
import autosize from 'autosize'

export default {
  props: {
    autosize: {
      type: Boolean,
      required: false,
      default: false
    },
    maxlength: {
      required: false
    },
    expandable: String,
    type: {
      type: String,
      default: 'text'
    },
    rows: {
      required: false
    },
    id: String,
    value: {
      required: false
    },
    disabled: {
      type: [Boolean, String],
      fill: true,
      required: false,
      default: false
    },
    required: {
      type: [Boolean, String],
      fill: true,
      required: false,
      default: false
    },
    readonly: {
      type: [Boolean, String],
      fill: true,
      required: false,
      default: false
    },
    label: String,
    pattern: String,
    error: String,
    textarea: Boolean,
    floatingLabel: [Boolean, String]
  },
  computed: {
    displayLabel () {
      return this.label || this.floatingLabel
    },
    isDirty () {
      return '' + this.value
    }
  },
  methods: {
    fireInputEvent: function (event) {
      this.$emit('input', event.target.value)
      this.$emit('originput', event)
    },
    fireChangeEvent: function (event) {
      this.$emit('change', event)
    },
    fireBlurEvent: function (event) {
      this.$emit('blur', event)
    },
    fireFocusEvent: function (event) {
      this.$emit('focus', event)
    },
    focus () {
      this.$refs.focusTarget.focus()
      return this
    },
    select () {
      this.$refs.focusTarget.select()
      return this
    },
    blur () {
      this.$refs.focusTarget.blur()
      return this
    }
  },
  mounted () {
    componentHandler.upgradeElement(this.$el)
    if (this.autosize && this.textarea) autosize(this.$el.querySelector('textarea'))
  }
}
</script>
