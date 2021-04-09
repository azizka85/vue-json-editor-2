<template>
  <b-container fluid>
    <div class="ml-2 mr-2 mb-2 mt-2 d-flex flex-lg-row flex-column-reverse">
      <div class="mr-lg-1 mt-2 mt-lg-0 col-lg-8 d-flex flex-column">
        <h5>JSON Output</h5>
        <p>
          Below the raw JSON {{ !showSchema ? 'data' : 'schema' }}
          <b-button 
            class="ml-2"
            size="sm"
            variant="outline-primary" 
            @click="toggleShowSchema"
          >
            Show {{ showSchema ? 'data' : 'schema' }}
          </b-button>
        </p>     
        <b-form-textarea 
          v-if="!showSchema"
          max-rows="1000" 
          :value="jsonData"
          class="flex-fill"
          @input="updateJsonData"
        />        
        <b-form-textarea 
          v-if="showSchema"
          max-rows="1000" 
          :value="jsonSchema"
          class="flex-fill"
          @input="updateJsonSchema"
        />
      </div>
      <div class="ml-lg-1 mb-2 mb-lg-0 col-lg-4">
        <h5>Editor</h5>
        <p>Below is the editor generated from the JSON data and schema</p>
        <template v-if="data">
          <b-form>
            <component 
              v-for="(value, key) in data"
              :key="key"
              :is="fieldSchemaType(key, value, schema)"
              :data="data"
              :schema="childSchema(key, schema)"
              :name="key"
              :value="value"
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
          </b-form>
        </template>
      </div>
    </div>   
  </b-container>  
</template>

<script>
import Vue from 'vue';
import { BContainer, BFormTextarea, BButton, BForm, BFormInput, BDropdown, BDropdownItemButton } from 'bootstrap-vue';

export default {
  name: 'Editor',
  data: () => ({
    showSchema: false,
    data: {
      user: {
        firstName: '',
        lastName: '',
        address: '',
        phone: [
          '999-999-999',
          '888-888-888'
        ]
      },
      notes: []
    },
    jsonData: '',
    schema: {
      user: {
        description: 'User info',
        title: "User"
      }
    },
    jsonSchema: ''
  }),
  methods: {
    toggleShowSchema() {
      this.showSchema = !this.showSchema;
    },
    addItem(item) {
      this.addItemInGroup(
        this.$refs.fieldNameInput.$el.value,
        item,
        this.data
      );
      this.setJsonData(this.data);
    },
    updateJsonData(json) {
      this.setJsonData(json);
      this.setData(json);
    },
    setJsonData(json) {
      let data = '';
      
      if(json) {
        if(typeof json === "object") {
          data = JSON.stringify(json, null, 2);
        } else {
          data = json;
        }        
      }

      this.jsonData = data;
    },
    setData(json) {
      let data = null;

      if(typeof json === "string") {
        try {
          data = JSON.parse(json);
        } catch(e) { }
      } else {
        data = json;
      }

      this.data = data;
    },
    updateJsonSchema(json) {
      this.setJsonSchema(json);
      this.setSchema(json);
    },
    setJsonSchema(json) {
      let data = '';
      
      if(json) {
        if(typeof json === "object") {
          data = JSON.stringify(json, null, 2);
        } else {
          data = json;
        }        
      }

      this.jsonSchema = data;
    },
    setSchema(json) {
      let data = null;

      if(typeof json === "string") {
        try {
          data = JSON.parse(json);
        } catch(e) { }
      } else {
        data = json;
      }

      this.schema = data;
    },
    childSchema(key, schema) {
      if(schema && typeof schema === 'object' && schema[key]) {
        return schema[key];
      } 

      return null;
    },
    fieldSchemaType(key, value, schema) {
      if(schema && typeof schema === 'object' && schema[key] && schema[key].type) {
        return schema[key].type;
      } else {
        const type = typeof value;

        switch(type) {
          case 'string':            
          case 'number':
          case 'bigint':
            return 'InputField';
          case 'boolean':
            return 'CheckboxField';
          case 'object':
            return Array.isArray(value) ? "ListField" : "GroupField";          
        }
      }

      return 'LiteralField';      
    },
    itemSchemaType(key, value, schema) {
      if(schema && typeof schema === 'object' && schema[key] && schema[key].type) {
        return schema[key].type;
      } else {
        const type = typeof value;

        switch(type) {
          case 'string':            
          case 'number':
          case 'bigint':
            return 'InputItem';
          case 'boolean':
            return 'CheckboxItem';
          case 'object':
            return Array.isArray(value) ? "ListItem" : "GroupItem";          
        }
      }

      return 'LiteralItem'; 
    },
    setFieldCollapsed(schema, collapsed) {
      Vue.set(schema, 'collapsed', collapsed);
    },
    setFieldValue(data, key, value) {      
      Vue.set(data, key, value);      
    },
    addItemInList(item, list) {
      list.push(item);
    },
    addItemInGroup(key, item, group) {
      Vue.set(group, key, item);
    }
  },
  mounted() {
    this.setJsonData(this.data);
    this.setData(this.data);

    this.setJsonSchema(this.schema);
    this.setSchema(this.schema);
  },
  provide: function() {
    return {
      data: this.data,
      jsonData: this.jsonData,
      schema: this.schema,
      jsonSchema: this.jsonSchema,
      setJsonData: this.setJsonData,
      setData: this.setData,
      setJsonSchema: this.setJsonSchema,
      setSchema: this.setSchema,
      childSchema: this.childSchema,
      fieldSchemaType: this.fieldSchemaType,
      itemSchemaType: this.itemSchemaType,
      setFieldCollapsed: this.setFieldCollapsed,
      setFieldValue: this.setFieldValue,
      addItemInList: this.addItemInList,
      addItemInGroup: this.addItemInGroup
    }
  },
  components: {
    BContainer, BFormTextarea, BButton, BForm, BFormInput, BDropdown, BDropdownItemButton,
    'LiteralField': () => import('./fields/LiteralField'),
    'InputField': () => import('./fields/InputField'),
    'CheckboxField': () => import('./fields/CheckboxField'),
    'GroupField': () => import('./fields/GroupField'),
    'ListField': () => import('./fields/ListField')
  }
}
</script>
