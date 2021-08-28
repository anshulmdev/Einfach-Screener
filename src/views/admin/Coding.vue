<template>
  <div class="flex flex-wrap m-1">
    <div class="w-full">
      <Code :method="hideComponents" :category="categoryCoding" />
    </div>
  </div>
</template>
<script>
import Code from "@/components/Assignment/Code.vue";

export default {
  props: {
    method: { type: Function },
  },
  components: {
    Code,
  },
  computed: {
    categoryCoding () {
      return this.$route.params.id
    }
  },
  methods: {
    hideComponents() {
      this.method()
    }
  }
};
</script>
