<template>
  <div class="mb-3 bg-light p-3">
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
  </div>  
</template>

<script>
export default {
  name: "GroupItem",
  inject: {
    childSchema: {
      from: 'childSchema',
      default: () => {}
    },
    itemSchemaType: {
      from: 'itemSchemaType',
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
  components: {
    'LiteralItem': () => import('./LiteralItem'),
    'InputItem': () => import('./InputItem'),
    'CheckboxItem': () => import('./CheckboxItem'),
    'ListItem': () => import('./ListItem')
  }  
}
</script>
