<template>
  <div class="search-box">
    <input type="text" placeholder="搜索" :value="value" @input="$emit('input', $event.target.value)">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "SearchBox",
  props: ["value"],
  mounted() {
    console.log(this.value);
  },
  watch: {
    value: function(newVal, oldVal) {
      console.log(`newVal: ${newVal}`);
    }
  }
};
</script>
