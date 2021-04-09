<template>
  <div class="mb-3">
    {{ value }}
  </div>
</template>

<script>
export default {
  name: "LiteralItem",
  data: () => ({
    category: 'content'
  }),
  props: {
    data: [Object, Array],
    schema: Object,
    field: String,
    name: String,
    value: String
  }  
}
</script>
