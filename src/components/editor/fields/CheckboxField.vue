<template>
  <div class="mb-3">
    <h6 class="expand-collapse" @click="toggleCollapse">
      <b-icon-chevron-down :rotate="collapsed ? -90 : 0" />
      {{ schema && schema.title ? schema.title : name }}
    </h6>
    <b-collapse :visible="!collapsed">     
      <div class="d-inline-flex align-items-center">
        <b-form-checkbox
          :id="'form-' + category + '-input-' + name"
          :checked="value"
          @input="onInput"
        >
          {{ schema && schema.label ? schema.label : name }}
        </b-form-checkbox>
        <b-icon-info-circle 
          v-if="schema && schema.description"
          :id="'form-' + category + '-icon-' + name" 
          class="ml-1"
        />
        <b-tooltip 
          v-if="schema && schema.description"
          :target="'form-' + category + '-icon-' + name"
          triggers="hover"
        >
          {{ schema.description }}
        </b-tooltip>         
      </div>                  
    </b-collapse>     
  </div>
</template>

<script>
import { BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip, BFormCheckbox } from 'bootstrap-vue';

export default {
  name: "CheckboxField",
  inject: {
    dataRoot: {
      from: 'data',
      default: null
    },
    schemaRoot: {
      from: 'schema',
      default: null
    },
    setJsonData: {
      from: 'setJsonData',
      default: () => {}
    },
    setJsonSchema: {
      from: 'setJsonSchema',
      default: () => {}
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
    value: Boolean
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
    BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip, BFormCheckbox
  }   
}
</script>
