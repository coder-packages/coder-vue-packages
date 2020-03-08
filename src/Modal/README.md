# Modal

- Note: Don’t forget to add `<portal-target>` for modal.
- You must install `npm install portal-vue`
- Add following into App.vue

```js
import PortalVue from ‘portal-vue’
Vue.use(PortalVue)
```

_Modal Usage example:_

```html
<template>
  <div>
    <modal
      :backdrop="true"
      :closeOnClickOutside="true"
      :closeOnEsc="false"
      v-model="showModal"
    >
      <div>
        My modal content here
      </div>
    </modal>

    <portal-target name="modals"></portal-target>
  </div>
</template>

<script type="text/javascript">
  import { Modal } from "coder-vue-components";
  import "node_modules/coder-vue-components/dist/coder-vue-components.css";

  export default {
    components: { Modal },
    data() {
      return {
        showModal: true
      };
    }
  };
</script>
```
