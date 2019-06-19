> 核💓代码：`$emit('input', $event.target.value)`

```vue
<!-- SearchBox.vue -->
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
```

```vue
<!-- App.vue -->
<template>
  <div class="home">
    <search-box v-model="searchText">
      <span>slot</span>
    </search-box>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      searchText: ""
    };
  }
};
</script>
```

```js
// main.js
import Vue from "vue";
import App from "./App.vue";
import SearchBox from "@/components/SearchBox.vue";

Vue.component("SearchBox", SearchBox); // 全局注册组件

new Vue({
  render: h => h(App)
}).$mount("#app");
```

## v-model 的本质

注意👀：Vue 内部做了处理 `$event === $event.target.value`

子组件的 `input` 事件被触发时通过 `$emit` 方法触发父组件的 `input` 方法达到修改父组件的值的目的🚝

```vue
<input v-model="searchText">

<!-- 等价 -->

<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>

<custom-input v-model="searchText"></custom-input>

<!-- 等价 -->

<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>

<script>
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
</script>
```

[https://vuejs.org/v2/guide/components.html#Emitting-a-Value-With-an-Event](https://vuejs.org/v2/guide/components.html#Emitting-a-Value-With-an-Event)

## an example

通过 `$emit` 关联父子组件的事件，当子组件的 `click` 事件被触发时通过 `$emit` 方法触发父组件的 `enlarge-text` 方法达到修改父组件的值的目的🚝

```vue
<!-- Child Component -->
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>

<!-- Parent Component -->
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>

<!-- or -->
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
<script>
export default {
  methods: {
    onEnlargeText: function (enlargeAmount) { // enlargeAmount == $event == 0.1
      this.postFontSize += enlargeAmount
    }
  }
}
</script>
```

[https://vuejs.org/v2/guide/components.html#Emitting-a-Value-With-an-Event](https://vuejs.org/v2/guide/components.html#Emitting-a-Value-With-an-Event)
