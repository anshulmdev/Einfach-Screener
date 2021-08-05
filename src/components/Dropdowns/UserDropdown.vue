<template>
  <div>
    <a
      class="text-blueGray-500 block"
      href="#pablo"
      ref="btnDropdownRef"
      v-on:click="toggleDropdown($event)"
    >
      <div class="items-center flex">
        <span
          class="w-12 h-12 text-sm text-white bg-blueGray-200 inline-flex items-center justify-center rounded-full"
        >
          <img
            alt="..."
            class="w-full rounded-full align-middle border-none shadow-lg"
            :src="image"
          />
        </span>
      </div>
    </a>
    <div
      ref="popoverDropdownRef"
      class="bg-white text-base z-50 float-left py-2 list-none text-left rounded shadow-lg min-w-48"
      v-bind:class="{
        hidden: !dropdownPopoverShow,
        block: dropdownPopoverShow,
      }"
    >
      <a
        @click="logout()"
        class="text-sm py-2 px-4 font-normal block w-full whitespace-nowrap bg-transparent text-blueGray-700 cursor-pointer"
      >
        Close Test
      </a>
    </div>
        <div v-if="loading">
      <link rel="stylesheet" href="https://pagecdn.io/lib/font-awesome/5.10.0-11/css/all.min.css" integrity="sha256-p9TTWD+813MlLaxMXMbTA7wN/ArzGyW/L7c5+KkjOkM=" crossorigin="anonymous" />

      <div class="w-full h-full fixed block top-0 left-0 bg-white opacity-75 z-50"  style="top: 50%; left: 50%">
        <span class="text-green-500 opacity-75 top-1/2 my-0 mx-auto block relative w-0 h-0">
          <i class="fas fa-circle-notch fa-spin fa-5x"></i>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import { createPopper } from "@popperjs/core";
import VueCookies from "vue-cookies";
import firebase from "../../firebase";
import CryptoJS from "crypto-js";

export default {
  data() {
    return {
      dropdownPopoverShow: false,
      loading:null,
      image:
        "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTqmn8-9Vb7FEOLeMEvW6DoLoRyMBP8joLEbg&usqp=CAU",
    };
  },
  props: {
    candidateData: Object,
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
    },
    async logout() {
      this.loading = true
      const candidateInfo = this.candidateData;
      const companyUid = CryptoJS.AES.decrypt(
        VueCookies.get("fbb3cu24"),
        "736b9960-fbb3-4430-a653-f9f4d58ddfe1"
      ).toString(CryptoJS.enc.Utf8);
      const snapshotData = await firebase
        .firestore()
        .collection("scores")
        .doc(companyUid)
        .get();
      // eslint-disable-next-line no-unused-vars
      const entry = await firebase
        .firestore()
        .collection("accounts")
        .doc(companyUid);
      const req = await entry.get();
      const companyData = req.data();
      const scoreInfo = snapshotData.data()[this.candidateData.email];
      let score = 0;
      Object.keys(scoreInfo).forEach((e) => {
        if (e === "coding") {
          Object.values(scoreInfo[e]).forEach((e)=>{
            score += e.score
          })
        } else {
          score += scoreInfo[e].score;
        }
      });
      candidateInfo.score = score;
      if (score > parseInt(companyData.assignment.cutoff)) {
        await entry.update({
          "candidates.shortlisted": firebase.firestore.FieldValue.arrayUnion(
            candidateInfo
          ),
        });
      } else {
        await entry.update({
          "candidates.rejected": firebase.firestore.FieldValue.arrayUnion(
            candidateInfo
          ),
        });
      }
      candidateInfo.score = 0;
      await entry.update({
        "candidates.ongoing": firebase.firestore.FieldValue.arrayRemove(
          candidateInfo
        ),
      });
      this.loading = null
      VueCookies.remove("fbb3em24");
      VueCookies.remove("fbb3cu24");
    },
  },
};
</script>
