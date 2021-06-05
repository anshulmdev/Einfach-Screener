<template>
  <div>
    <div class="container mx-auto px-4 h-full ">
      <div
        v-if="formData"
        class="flex content-center items-center justify-center h-full"
      >
        <div class="w-full px-4">
          <div class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-sm bg-white border-0">
                  aaaaaaaaa
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { DB } from "../../firebase";
export default {
  layout: "LoginLayout",
  data() {
    return {
      formData: null,
    };
  },
  methods: {
    async fetchRtdb() {
      try {
        const data = await DB.ref(`forms/${this.$route.params.id}`);
        data
          .get()
          .then((snapshot) => {
            if (snapshot.exists()) {
              this.formData = snapshot.val();
            } else {
              console.log("No data available");
            }
          })
          .catch((error) => {
            console.error(error);
          });
      } catch (err) {
        alert(err.message);
      }
      this.loading = null;
    },
  },
  created() {
    this.fetchRtdb();
  },
};
</script>