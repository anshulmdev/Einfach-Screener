<template>
  <div>
    <sidebar :companyData="companyFetchedData" :candidateData="candidateDatabase" />
    <div class="relative md:ml-64 bg-blueGray-100">
      <admin-navbar :seconds="secondsLeft" :candidateData="candidateDatabase" />
      <header-stats :companyData="companyFetchedData" :candidate="candidateDatabase" />
      <div class="px-4 md:px-10 mx-auto w-full -m-24">
        <router-view />
        <footer-admin />
      </div>
    </div>
    <div v-if="loading">
      <link rel="stylesheet" href="https://pagecdn.io/lib/font-awesome/5.10.0-11/css/all.min.css" integrity="sha256-p9TTWD+813MlLaxMXMbTA7wN/ArzGyW/L7c5+KkjOkM=" crossorigin="anonymous">

<div class="w-full h-full fixed block top-0 left-0 bg-white opacity-75 z-50">
  <span class="text-green-500 opacity-75 top-1/2 my-0 mx-auto block relative w-0 h-0" style="
    top: 50%;
">
    <i class="fas fa-circle-notch fa-spin fa-5x"></i>
  </span>
</div>
      </div>
  </div>
</template>
<script>
import AdminNavbar from "@/components/Navbars/AdminNavbar.vue";
import Sidebar from "@/components/Sidebar/Sidebar.vue";
import HeaderStats from "@/components/Headers/HeaderStats.vue";
import FooterAdmin from "@/components/Footers/FooterAdmin.vue";
import VueCookies from "vue-cookies";
import firebase from "../firebase";
import CryptoJS from "crypto-js";
export default {
  name: "admin-layout",
  data() {
    return {
      secondsLeft: "",
      companyFetchedData: null,
      candidateDatabase: null,
      loading: null
    };
  },
  components: {
    AdminNavbar,
    Sidebar,
    HeaderStats,
    FooterAdmin,
  },
  methods: {
    async startTimer() {
      const secondsToHms = function (d) {
        d = Number(d);
        var h = Math.floor(d / 3600);
        var m = Math.floor((d % 3600) / 60);
        var s = Math.floor((d % 3600) % 60);

        var hDisplay = h > 0 ? h + (h == 1 ? " hour, " : " hours, ") : "";
        var mDisplay = m > 0 ? m + (m == 1 ? " minute, " : " minutes, ") : "";
        var sDisplay = s > 0 ? s + (s == 1 ? " second" : " seconds") : "";
        return hDisplay + mDisplay + sDisplay;
      };
      if (VueCookies.get("fbb3cu24") && VueCookies.get("fbb3em24")) {
        const email = CryptoJS.AES.decrypt(VueCookies.get("fbb3em24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8);
        const companyUid = CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8);
        const snapshotData = await firebase
          .firestore()
          .collection("accounts")
          .doc(companyUid);
        // eslint-disable-next-line no-unused-vars
        const observer = snapshotData.onSnapshot(
          (snapshot) => {
            const data = snapshot.data();
            const companyData = data.candidates.ongoing;
            this.companyFetchedData = data;
            const assignment = data.assignment;
            let ongoingValid = false;
            let candidateData = null;
            companyData.forEach((candidate) => {
              if (candidate.email === email) {
                ongoingValid = true;
                candidateData = candidate;
              }
            });
            this.candidateDatabase = candidateData
            if (!ongoingValid) {
              this.$router.push({ path: "/" });
            } else {
              setInterval(() => {
                const loginTime = new Date(candidateData.timeStamp);
                const currentTime = new Date();
                const seconds = parseInt(
                  (currentTime.getTime() - loginTime.getTime()) / 1000
                );
                const testDuration = parseInt(assignment.time) * 60;
                if((testDuration - seconds) < 0) {
                  VueCookies.remove("fbb3em24")
                  VueCookies.remove("fbb3cu24")
                  this.$router.push({ path: "/" });
                }
                this.secondsLeft = secondsToHms(testDuration - seconds);
              }, 1000);
            }
            // ...
          },
          (err) => {
            console.log(`Encountered error: ${err}`);
            VueCookies.remove("fbb3em24")
            VueCookies.remove("fbb3cu24")
            this.$router.push({ path: "/" });
          }
        );
      } else {
        this.$router.push({
          path: "/",
        });
      }
      this.loading = null
    },
  },
  created() {
    this.loading = true
    this.startTimer();
  },
};
</script>
