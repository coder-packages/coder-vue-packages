# ClickOutside

Component for handle some event on click outside of content of that component

_ClickOutside Usage example:_

```html
<template>
  <div>
    <click-outside :do="handleOnClickOutside">
      <div>
        This is my content. On click outside of my content will trigger
        handleOnClickOutside function
      </div>
    </click-outside>
  </div>
</template>

<script type="text/javascript">
  import { ClickOutside } from "coder-vue-components";

  export default {
    components: { ClickOutside },
    methods: {
      handleOnClickOutside() {
        console.log("click outside");
      }
    }
  };
</script>
```
