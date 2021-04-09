<template>
  <div class="mb-2">
    <component 
      v-for="(item, index) in value"
      :key="index"
      :is="itemSchemaType(index, item, schema ? schema.items : null)"
      :data="value"
      :schema="childSchema(index, schema ? schema.items : null)"
      :field="field + '.' + name"
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
  </div>
</template>

<script>
import { BDropdown, BDropdownItemButton } from 'bootstrap-vue';

export default {
  name: "ListItem",
  inject: {
    dataRoot: {
      from: 'data',
      default: null
    },
    setJsonData: {
      from: 'setJsonData',
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
    addItemInList: {
      from: 'addItemInList',
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
    value: Array
  },
  methods: {
    addItem(item) {
      this.addItemInList(item, this.value);
      this.setJsonData(this.dataRoot);
    }
  },
  components: {
    BDropdown, BDropdownItemButton,
    'LiteralItem': () => import('./LiteralItem'),
    'InputItem': () => import('./InputItem'),
    'CheckboxItem': () => import('./CheckboxItem'),
    'GroupItem': () => import('./GroupItem')
  }
}
</script>
