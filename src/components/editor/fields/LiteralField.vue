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
      {{ value }}                             
    </b-collapse>
  </div>
</template>

<script>
import { BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip } from 'bootstrap-vue';

export default {
  name: "LiteralField",
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
    }
  },
  components: {
    BIconChevronDown, BIconInfoCircle, BCollapse, BTooltip,
  }
}
</script>
