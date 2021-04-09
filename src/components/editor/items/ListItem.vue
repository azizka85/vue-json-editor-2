<template>
  <div class="mb-3">
    <component 
      v-for="(item, index) in value"
      :key="index"
      :is="itemSchemaType(index, item, schema ? schema.child : null)"
      :data="value"
      :schema="childSchema(index, schema ? schema.child : null)"
      :field="field + '.' + name"
      :name="'' + index"
      :value="item"
    />    
  </div>
</template>

<script>
export default {
  name: "ListItem",
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
    value: Array
  },
  components: {
    'LiteralItem': () => import('./LiteralItem'),
    'InputItem': () => import('./InputItem'),
    'CheckboxItem': () => import('./CheckboxItem'),
    'GroupItem': () => import('./GroupItem')
  }
}
</script>
