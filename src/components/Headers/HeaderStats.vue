<template>
  <!-- Header -->
  <div v-if="companyData" class="relative bg-emerald-600 md:pt-32 pb-32 pt-12">
    <div class="px-4 md:px-10 mx-auto w-full">
      <div>
        <!-- Card stats -->
        <div class="flex flex-wrap">
          <div v-for="(item, index) in headerData" :key="index" class="w-full lg:w-6/12 xl:w-3/12 px-4">
            <card-stats
              :statSubtitle="item.name"
              :statTitle="`${item.total} Questions`"
              statArrow="up"
              :statPercent="item.score"
              statPercentColor="text-emerald-500"
              statDescripiron="questions attempted"
              :statIconName="`fas fa-${item.icon}`"
              :statIconColor="item.color"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardStats from "@/components/Cards/CardStats.vue";
import firebase from "../../firebase";
import VueCookies from "vue-cookies";
import CryptoJS from "crypto-js";

export default {
  components: {
    CardStats,
  },
  data () {
    return {
      scores: {}
    }
  },
  props: {
    companyData: Object,
    candidate: Object
  },
  created () {
    this.fetchScore()
  },
  methods: {
    async fetchScore () {
      const req = await firebase.firestore().collection('scores').doc(CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8))
      await req.onSnapshot(
          (snapshot) => {
            this.scores = snapshot.data()[CryptoJS.AES.decrypt(VueCookies.get("fbb3em24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)];})
    }
  },
  computed: {
    headerData () {
      const assignments = this.companyData.assignment.tags
      const generalArray = []
      let codingTotal = 0
      Object.keys(assignments).forEach((e) => {
        if (e === 'mcq') generalArray.push({name: 'MCQs', total: assignments[e].length, icon: 'list', color: 'bg-pink-500'
        , score: this.scores && this.scores.mcq ? this.scores.mcq.attempted : 0})
        if (e === 'regex') generalArray.push({name: 'Regex', total: assignments[e].length, icon: 'fingerprint'
        , color: 'bg-red-500', score: this.scores && this.scores.regex ? this.scores.regex.attempted : 0})
        if (e === 'rest') generalArray.push({name: 'REST APIs', total: assignments[e].length, icon: 'cloud-upload-alt'
        , color: 'bg-orange-500', score: this.scores && this.scores.rest ? this.scores.rest.attempted : 0})
        if (e === 'array' || e === 'dp') {
          codingTotal += assignments[e].length
        }
      })
      generalArray.push({name: 'Programming', total: codingTotal, icon: 'code', color: 'bg-emerald-500'
      , score: this.scores && this.scores.coding ? Object.keys(this.scores.coding).length : 0})
      return generalArray.sort((a, b) => a.name.localeCompare(b.name))
    }
  }
};
</script>
