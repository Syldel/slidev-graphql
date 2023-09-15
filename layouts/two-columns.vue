<!--
  Usage:

```md
---
layout: two-columns
---

# Left

This shows on the left

::right::

# Right

This shows on the right
```

-->

<script setup lang="ts">
import { computed } from 'vue'
import { handleBackground } from '../node_modules/slidev-theme-the-unnamed/utils/layoutHelper'
const props = defineProps({
  class: {
    type: String,
  },
  layoutClass: {
    type: String,
  },
  background: {
    default: '',
  },
})

const style = computed(() => handleBackground(props.background, true))
</script>

<template>
  <div class="slidev-layout two-columns w-full h-full grid grid-cols-2" :class="props.layoutClass" :style="style">
    <div class="col-left" :class="props.class">
      <slot />
    </div>
    <div class="col-right" :class="props.class">
      <slot name="right" />
    </div>
  </div>
</template>
