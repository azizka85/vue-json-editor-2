<template>
  <b-form-input 
    class="mb-2"
    :placeholder="schema && schema.placeholder ? schema.placeholder : name"
    :type="schema && schema.format ? schema.format : 'text'"
    :value="value"
    @input="onInput"
  />
</template>

<script>
import { BFormInput } from 'bootstrap-vue';

export default {
  name: "InputItem",
  inject: {
    dataRoot: {
      from: 'data',
      default: () => {}
    },
    setFieldValue: {
      from: 'setFieldValue',
      default: () => {}
    },
    setJsonData: {
      from: 'setJsonData',
      default: () => {}
    }
  },
  data: () => ({
    category: 'content'
  }),
  props: {
    data: [Object, Array],
    schema: Object,
    field: String,
    name: String,
    value: [String, Number, BigInt, null]
  },
  methods: {
    onInput(value) {
      this.setFieldValue(this.data, this.name, value);
      this.setJsonData(this.dataRoot);
    }
  },
  components: {
    BFormInput
  }
}
</script>
