<template>
  <div class="mb-2 bg-light p-3">
    <component 
      v-for="(item, key) in value"
      :key="key"
      :is="itemSchemaType(key, item, schema ? schema.items : null)"
      :data="value"
      :schema="childSchema(key, schema ? schema.items : null)"
      :field="field + '.' + name"
      :name="key"
      :value="item"
    />    
    <div class="d-inline-flex">
      <b-form-input
        ref="fieldNameInput"          
        placeholder="Field name" 
        class="mr-2"         
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
  </div>  
</template>

<script>
import { BFormInput, BDropdown, BDropdownItemButton } from 'bootstrap-vue';

export default {
  name: "GroupItem",
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
    addItemInGroup: {
      from: 'addItemInGroup',
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
    value: Object
  },
  methods: {
    addItem(item) {
      this.addItemInGroup(
        this.$refs.fieldNameInput.$el.value,
        item,
        this.value
      );
      this.setJsonData(this.dataRoot);
    }    
  },
  components: {
    BFormInput, BDropdown, BDropdownItemButton,
    'LiteralItem': () => import('./LiteralItem'),
    'InputItem': () => import('./InputItem'),
    'CheckboxItem': () => import('./CheckboxItem'),
    'ListItem': () => import('./ListItem')
  }  
}
</script>
