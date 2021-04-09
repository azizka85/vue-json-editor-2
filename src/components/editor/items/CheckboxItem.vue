<template>
  <b-form-checkbox
    class="mb-2"
    :checked="value"
    @input="onInput"  
  >
    {{ schema && schema.label ? schema.label : name }}
  </b-form-checkbox>
</template>

<script>
import { BFormCheckbox } from 'bootstrap-vue';

export default {
  name: "CheckboxItem",
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
    value: Boolean
  },
  methods: {
    onInput(value) {
      this.setFieldValue(this.data, this.name, value);
      this.setJsonData(this.dataRoot);
    }
  },
  components: {
    BFormCheckbox
  }  
}
</script>
