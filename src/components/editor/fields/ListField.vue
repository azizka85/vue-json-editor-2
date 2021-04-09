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
        <component 
          v-for="(item, index) in value"
          :key="index"
          :is="itemSchemaType(index, item, schema ? schema.items : null)"
          :data="value"
          :schema="childSchema(index, schema ? schema.items : null)"
          :field="name"
          :name="'' + index"
          :value="item"
        />
        <b-dropdown 
          size="sm" 
          variant="outline-primary" 
          text="Add item"
        >     
          <b-dropdown-item-button 
            @click="addItem('')"
          >
            Input item
          </b-dropdown-item-button>
          <b-dropdown-item-button 
            @click="addItem(false)"
          >
            Checkbox item
          </b-dropdown-item-button>
          <b-dropdown-item-button 
            @click="addItem([])"
          >
            List item
          </b-dropdown-item-button>
          <b-dropdown-item-button
            @click="addItem({})"
          >
            Group item
          </b-dropdown-item-button>
        </b-dropdown>   
    </b-collapse>
  </div>  
</template>

<script>
import { BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip, BDropdown, BDropdownItemButton } from 'bootstrap-vue';

export default {
  name: "ListField",
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
    },
    addItemInList: {
      from: 'addItemInList',
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
    value: Array
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
    },
    addItem(item) {
      this.addItemInList(item, this.value);
      this.setJsonData(this.dataRoot);
    }
  },
  components: {
    BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip, BDropdown, BDropdownItemButton,
    'LiteralItem': () => import('../items/LiteralItem'),
    'InputItem': () => import('../items/InputItem'),
    'CheckboxItem': () => import('../items/CheckboxItem'),
    'ListItem': () => import('../items/ListItem'),
    'GroupItem': () => import('../items/GroupItem')
  }     
}
</script>
