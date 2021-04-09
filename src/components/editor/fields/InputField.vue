<template>
  <div class="mb-3">
    <h6 class="expand-collapse" @click="toggleCollapse">
      <b-icon-chevron-down :rotate="collapsed ? -90 : 0" />
      {{ schema && schema.title ? schema.title : name }}
    </h6>
    <b-collapse :visible="!collapsed">
      <b-tooltip 
        v-if="schema && schema.description"
        :target="'form-' + category + '-icon-' + name"
        triggers="hover"
      >
        {{ schema.description }}
      </b-tooltip>
      <b-icon-info-circle 
        v-if="schema && schema.description"
        :id="'form-' + category + '-icon-' + name" 
      />
      <label :for="'form-' + category + '-input-' + name" class="ml-2">
        {{ schema && schema.label ? schema.label : name }}:
      </label>
      <b-form-input
        :id="'form-' + category + '-input-' + name"
        :placeholder="schema && schema.placeholder ? schema.placeholder : ''"
        :type="schema && schema.format ? schema.format : 'text'"
        :value="value"
        @input="onInput"
      />
    </b-collapse>     
  </div>
</template>

<script>
import { BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip, BFormInput } from 'bootstrap-vue';

export default {
  name: "InputField",
  inject: {
    schemaRoot: {
      from: 'schema',
      default: null
    },
    dataRoot: {
      from: 'data',
      default: null
    },
    setJsonData: {
      from: 'setJsonData',
      default: () => {}
    },
    setJsonSchema: {
      from: 'setJsonSchema',
      default: () => {},
    },
    setFieldValue: {
      from: 'setFieldValue',
      default: () => {}
    },
    setFieldCollapsed: {
      from: 'setFieldCollapsed',
      default: () => {}
    }
  },
  data: () => ({
    isCollapsed: false,
    category: 'content'
  }),  
  props: {
    data: Object,
    schema: Object,
    name: String,
    value: [String, Number, BigInt, null]
  },
  computed: {
    collapsed() {
      return this.schema ? this.schema.collapsed : this.isCollapsed;
    }    
  },
  methods: {
    onInput(value) {
      this.setFieldValue(this.data, this.name, value);
      this.setJsonData(this.dataRoot);
    },
    toggleCollapse() {
      if(this.schema) {
        this.setFieldCollapsed(this.schema, !this.collapsed);
        this.setJsonSchema(this.schemaRoot);
      } else {
        this.isCollapsed = !this.isCollapsed;
      }
    }    
  },
  components: {
    BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip, BFormInput
  }
}
</script>
