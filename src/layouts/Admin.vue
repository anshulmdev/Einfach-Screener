<template>
  <div>
    <sidebar :companyData="companyFetchedData" />
    <div class="relative md:ml-64 bg-blueGray-100">
      <admin-navbar :seconds="secondsLeft" />
      <header-stats :companyData="companyFetchedData" :candidate="candidateDatabase" />
      <div class="px-4 md:px-10 mx-auto w-full -m-24">
        <router-view />
        <footer-admin />
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
export default {
  name: "admin-layout",
  data() {
    return {
      secondsLeft: "",
      companyFetchedData: null,
      candidateDatabase: null
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
      VueCookies.set("companyUid", "XJNf2GucFiv4uwJULqlr")
      VueCookies.set("applicantEmail", "anshulm.dev@gmail.com")
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
      if (VueCookies.get("companyUid") && VueCookies.get("applicantEmail")) {
        const email = VueCookies.get("applicantEmail");
        const companyUid = VueCookies.get("companyUid");
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
                this.secondsLeft = secondsToHms(testDuration - seconds);
              }, 1000);
            }
            // ...
          },
          (err) => {
            console.log(`Encountered error: ${err}`);
            this.$router.push({ path: "/" });
          }
        );
      } else {
        this.$router.push({
          path: "/",
        });
      }
    },
  },
  created() {
    this.startTimer();
  },
};
</script>
