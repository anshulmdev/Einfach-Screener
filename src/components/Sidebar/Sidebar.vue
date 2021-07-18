<template>
  <nav v-if="companyData" class="md:left-0 md:block md:fixed md:top-0 md:bottom-0 md:overflow-y-auto md:flex-row md:flex-nowrap md:overflow-hidden shadow-xl bg-white flex flex-wrap items-center justify-between relative md:w-64 z-10 py-4 px-6">
    <div class="md:flex-col md:items-stretch md:min-h-full md:flex-nowrap px-0 flex flex-wrap items-center justify-between w-full mx-auto">
      <!-- Brand -->
      <router-link class="md:block text-left text-blueGray-600 mr-0 inline-block whitespace-nowrap text-sm uppercase font-bold p-2 px-0" to="/"> {{ companyData.user.name }} Test </router-link>
      <div class="md:flex md:flex-col md:items-stretch md:opacity-100 md:relative md:mt-4 md:shadow-none shadow absolute top-0 left-0 right-0 z-40 overflow-y-auto overflow-x-hidden h-auto items-center flex-1 rounded" v-bind:class="collapseShow">
        <hr class="mb-4 md:min-w-full" />
        <ul class="md:flex-col md:min-w-full flex flex-col list-none" v-for="(item, index) in general" :key="index">
          <li class="items-center">
            <router-link :to="`/admin/${item.path}`" v-slot="{ href, navigate, isActive }">
              <a :href="href" @click="navigate" class="text-xs uppercase py-3 font-bold block" :class="[isActive ? 'text-emerald-500 hover:text-emerald-600' : 'text-blueGray-700 hover:text-blueGray-500']">
                <i class="mr-2 text-sm" :class="`fas fa-${item.icon}`"></i>
                {{ item.name }}
              </a>
            </router-link>
          </li>
        </ul>
        <!-- Divider -->
        <hr class="my-4 md:min-w-full" />
        <!-- Heading -->
        <h6 v-if="codingRound.length" class="md:min-w-full text-blueGray-500 text-xs uppercase font-bold block pb-4 no-underline">Coding Section</h6>
        <!-- Navigation -->

        <ul class="md:flex-col md:min-w-full flex flex-col list-none" v-for="(item, index) in codingRound" :key="index">
          <li class="items-center">
            <router-link :to="`/admin/coding/${item.path}`" v-slot="{ href, navigate, isActive }">
              <a :href="href" @click="navigate" class="text-xs uppercase py-3 font-bold block" :class="[isActive ? 'text-emerald-500 hover:text-emerald-600' : 'text-blueGray-700 hover:text-blueGray-500']">
                <i class="mr-2 text-sm" :class="`fas fa-${item.icon}`"></i>
                {{ item.name }} {{ index }}
              </a>
            </router-link>
          </li>
        </ul>

        <!-- Divider -->
        <hr class="my-4 md:min-w-full" />
        <!-- Heading -->
        <h6 class="md:min-w-full text-blueGray-500 text-xs uppercase font-bold block pb-4 no-underline">Support</h6>
        <!-- Navigation -->

        <ul class="md:flex-col md:min-w-full flex flex-col list-none">
          <li class="items-center">
            <router-link to="/admin/ticket" v-slot="{ href, navigate, isActive }">
              <a :href="href" @click="navigate" class="text-xs uppercase py-3 font-bold block" :class="[isActive ? 'text-emerald-500 hover:text-emerald-600' : 'text-blueGray-700 hover:text-blueGray-500']">
                <i class="fas fa-ticket-alt mr-2 text-sm" :class="[isActive ? 'opacity-75' : 'text-blueGray-300']"></i>
                Raise Ticket
              </a>
            </router-link>
          </li>

          <li class="items-center">
            <a @click="logout()" class="text-xs uppercase py-3 font-bold block cursor-pointer">
              <i class="fas fa-sign-out-alt mr-2 text-sm"></i>
              Close Test
            </a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>
); }

<script>
import VueCookies from "vue-cookies"
import firebase from "../../firebase"
import CryptoJS from "crypto-js"

export default {
  data() {
    return {
      collapseShow: "hidden",
    }
  },
  props: {
    companyData: Object,
    candidateData: Object,
  },
  computed: {
    general() {
      const categories = Object.keys(this.companyData.assignment.tags)
      const generalArray = []
      categories.forEach((e) => {
        if (e === "mcq") generalArray.push({ name: "MCQs", path: "mcq", icon: "list" })
        if (e === "regex")
          generalArray.push({
            name: "Regex Pattern",
            path: "regex",
            icon: "fingerprint",
          })
        if (e === "rest")
          generalArray.push({
            name: "REST APIs",
            path: "rest",
            icon: "cloud-upload-alt",
          })
      })
      return generalArray.sort((a, b) => a.name.localeCompare(b.name))
    },
    codingRound() {
      const categories = Object.keys(this.companyData.assignment.tags)
      const generalArray = []
      categories.forEach((e) => {
        if (e === "array") generalArray.push({ name: "Question Set", path: "array", icon: "code" })
        if (e === "dp")
          generalArray.push({
            name: "Question Set",
            path: "dp",
            icon: "code",
          })
      })
      return generalArray.sort((a, b) => a.name.localeCompare(b.name))
    },
  },
  methods: {
    toggleCollapseShow: function (classes) {
      this.collapseShow = classes
    },
    async logout() {
      const candidateInfo = this.candidateData
      const companyUid = CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)
      const snapshotData = await firebase.firestore().collection("scores").doc(companyUid).get()
      // eslint-disable-next-line no-unused-vars
      const entry = await firebase.firestore().collection("accounts").doc(companyUid)
      const scoreInfo = snapshotData.data()[this.candidateData.email]
      let score = 0
      Object.keys(scoreInfo).forEach((e) => {
        if (e === "coding") {
          Object.values(scoreInfo[e]).forEach((e)=>{
            score += e.score
          })
        } else {
          score += scoreInfo[e].score
        }
      })
      candidateInfo.score = score
      if (score > parseInt(this.companyData.assignment.cutoff)) {
        await entry.update({
          "candidates.shortlisted": firebase.firestore.FieldValue.arrayUnion(candidateInfo),
        })
      } else {
        await entry.update({
          "candidates.rejected": firebase.firestore.FieldValue.arrayUnion(candidateInfo),
        })
      }
      candidateInfo.score = 0
      await entry.update({
        "candidates.ongoing": firebase.firestore.FieldValue.arrayRemove(candidateInfo),
      })
      VueCookies.remove("fbb3em24")
      VueCookies.remove("fbb3cu24")
    },
  },
}
</script>
