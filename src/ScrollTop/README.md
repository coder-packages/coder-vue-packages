# ScrollTop

Button for scrolling page to top.

Parameter showAfter need to show button if pages scrolled greater than showAfter px.

_ScrollTop Usage example:_

```html
<template>
  <div>
    <scroll-top :showAfter="150" />
  </div>
</template>

<script type="text/javascript">
  import { ScrollTop } from "coder-vue-components";

  export default {
    components: { ScrollTop }
  };
</script>
```
