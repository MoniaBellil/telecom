// example-vue-component.vue
<template>
  <button @click="showAlert">Hello world</button>
</template>

<script>
import Vue from "vue";
import Swal from "sweetalert2";

export default {
  methods: {
    showAlert() {
      // Use sweetalert2
      Swal.fire({
        icon: 'error',
        text: 'Liaison est down',
      })
    },
  },
};
</script>
