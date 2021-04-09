<template>
  <div class="mb-3">
    <h6 class="expand-collapse" @click="toggleCollapse">
      <b-icon-chevron-down :rotate="collapsed ? -90 : 0" />
      {{ schema && schema.title ? schema.title : name }}
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
    </h6>
    <b-collapse :visible="!collapsed">     
      <div class="bg-light p-3">
        <component 
          v-for="(item, key) in value"
          :key="key"
          :is="itemSchemaType(key, item, schema ? schema.items : null)"
          :data="value"
          :schema="childSchema(key, schema ? schema.items : null)"
          :field="name"
          :name="key"
          :value="item"
        />
      </div>                              
    </b-collapse>
  </div>
</template>

<script>
import { BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip } from 'bootstrap-vue';

export default {
  name: "GroupField",
  inject: {
    schemaRoot: {
      from: 'schema',
      default: null
    },
    setJsonSchema: {
      from: 'setJsonSchema',
      default: () => {}
    },
    childSchema: {
      from: 'childSchema',
      default: () => {}
    },
    itemSchemaType: {
      from: 'itemSchemaType',
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
    value: Object
  },
  computed: {
    collapsed() {
      return this.schema ? this.schema.collapsed : this.isCollapsed;
    }
  },
  methods: {
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
    BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip,
    'LiteralItem': () => import('../items/LiteralItem'),
    'InputItem': () => import('../items/InputItem'),
    'CheckboxItem': () => import('../items/CheckboxItem'),
    'ListItem': () => import('../items/ListItem'),
    'GroupItem': () => import('../items/GroupItem')
  }  
}
</script>
