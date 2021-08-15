<template>
  <div v-if="languages">
    <select v-model="languageSelected" class="py-1 px-1 rounded-md mt-1 text-black">
      <option v-for="item in languages" :key="item">{{item.name}}</option>
    </select>
  </div>
</template>

<script>
import { createPopper } from "@popperjs/core";

export default {
  data() {
    return {
      dropdownPopoverShow: false,
      languages: [{id: 63, name: "JavaScript (Node.js 12.14.0)"}, {id: 71, name: "Python (3.8.1)"}],
      languageSelected: 'JavaScript (Node.js 12.14.0)'
    };
  },
  methods: {
    toggleDropdown: function (event) {
      event.preventDefault();
      if (this.dropdownPopoverShow) {
        this.dropdownPopoverShow = false;
      } else {
        this.dropdownPopoverShow = true;
        createPopper(this.$refs.btnDropdownRef, this.$refs.popoverDropdownRef, {
          placement: "bottom-start",
        });
      }
    }
  }
};
</script>
