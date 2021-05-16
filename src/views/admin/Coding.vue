<template>
  <div class="flex flex-wrap mt-4">
    <div class="w-full mb-12 px-4">
      <Code :category="categoryCoding" />
    </div>
  </div>
</template>
<script>
import Code from "@/components/Assignment/Code.vue";

export default {
  components: {
    Code,
  },
  computed: {
    categoryCoding () {
      return this.$route.params.id
    }
  }
};
</script>
