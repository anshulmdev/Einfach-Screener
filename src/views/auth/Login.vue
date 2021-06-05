<template>
<div>
  <div class="container mx-auto px-4 h-full">
    <div
      v-if="companyData && !eligible"
      class="flex content-center items-center justify-center h-full"
    >
      <div class="w-full lg:w-4/12 px-4">
        <div
          class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-blueGray-200 border-0"
        >
          <div class="rounded-t mb-0 px-6 py-6">
            <div class="text-center mb-3">
              <h3 class="text-black-500 text-xl font-bold">
                {{ companyData.user.name }}
              </h3>
            </div>
            <hr class="mt-6 border-b-1 border-blueGray-300" />
          </div>
          <div class="flex-auto px-4 lg:px-10 py-10 pt-0">
            <div class="text-blueGray-400 text-center mb-3 font-bold">
              <small>Email Invitation Verification</small>
            </div>
            <form>
              <div class="relative w-full mb-3">
                <label
                  class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                  htmlFor="grid-password"
                >
                  Email
                </label>
                <input
                  v-model="form.email"
                  type="email"
                  class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                  placeholder="Email"
                />
              </div>

              <div class="text-center mt-6">
                <button
                  @click="signup"
                  class="bg-blueGray-800 text-white active:bg-blueGray-600 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full ease-linear transition-all duration-150"
                  type="button"
                >
                  Verify
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div
      v-if="companyData && eligible"
      class="flex content-center items-center justify-center h-full"
    >
      <div class="w-full lg:w-6/12 px-4">
        <div
          class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-blueGray-200 border-0"
        >
                    <div class="rounded-t mb-0 px-6 py-6">
            <div class="text-center mb-3">
              <h6 class="text-blueGray-500 text-sm font-bold">
                Verification of Details
              </h6>
            </div>
            <div class="btn-wrapper text-center">
              <button
                @click="grantPermissions('video')" :class="permissions.video ? 'bg-emerald-500' : 'bg-yellow-500'"
                class="px-4 py-2 rounded outline-none focus:outline-none mr-2 mb-1 uppercase shadow hover:shadow-md inline-flex items-center font-bold text-xs ease-linear transition-all duration-150"
                type="button"
              >
                <img alt="..." class="w-5 mr-1" src="@/assets/img/video.png" />
                Video
              </button>
              <button
                @click="grantPermissions('audio')" :class="permissions.audio ? 'bg-emerald-500' : 'bg-yellow-500'"
                class="px-4 py-2 rounded outline-none focus:outline-none mr-1 mb-1 uppercase shadow hover:shadow-md inline-flex items-center font-bold text-xs ease-linear transition-all duration-150"
                type="button"
              >
                <img alt="..." class="w-5 mr-1" src="@/assets/img/speaker.svg" />
                Audio
              </button>
            </div>
            <hr class="mt-6 border-b-1 border-blueGray-300" />
          </div>
          <div class="flex-auto px-4 lg:px-10 py-10 pt-0">
            <form>
              <div class="relative w-full mb-3">
                <label
                  class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                  htmlFor="grid-password"
                >
                  Name
                </label>
                <input
                  type="text"
                  v-model="candidateData.name"
                  class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                  placeholder="Name"
                />
              </div>

              <div class="relative w-full mb-3">
                <label
                  class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                  htmlFor="grid-password"
                >
                  Email
                </label>
                <input
                  type="text"
                  v-model="candidateData.email"
                  class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                  placeholder="Email"
                />
              </div>

              <div class="relative w-full mb-3">
                <label
                  class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                  htmlFor="grid-password"
                >
                  Phone
                </label>
                <input
                  type="text"
                  v-model="candidateData.phone"
                  class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                  placeholder="Phone"
                />
              </div>

              <div>
                <label class="inline-flex items-center cursor-pointer">
                  <input
                    id="customCheckLogin"
                    type="checkbox"
                    class="form-checkbox border-0 rounded text-blueGray-700 ml-1 w-5 h-5 ease-linear transition-all duration-150"
                  />
                  <span class="ml-2 text-sm font-semibold text-blueGray-600">
                    I agree with the
                    <a href="javascript:void(0)" class="text-emerald-500">
                      Privacy Policy
                    </a>
                  </span>
                </label>
              </div>

              <div class="text-center mt-6">
                <button
                  class="bg-blueGray-800 text-white active:bg-blueGray-600 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full ease-linear transition-all duration-150"
                  type="button"
                  @click="startTest"
                >
                  Start Test
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="container mx-auto px-4 h-full">
    <div
      class="flex content-center items-center justify-center h-full"
    >
              
    <div v-if="loading">
      <link rel="stylesheet" href="https://pagecdn.io/lib/font-awesome/5.10.0-11/css/all.min.css" integrity="sha256-p9TTWD+813MlLaxMXMbTA7wN/ArzGyW/L7c5+KkjOkM=" crossorigin="anonymous">

<div class="w-full h-full fixed block top-0 left-0 bg-white opacity-75 z-50">
  <span class="text-green-500 opacity-75 top-1/2 my-0 mx-auto block relative w-0 h-0" style="
    top: 50%; left:50%
">
    <i class="fas fa-circle-notch fa-spin fa-5x"></i>
  </span>
</div>
      </div>
      <div v-if="!companyData" class="w-full lg:w-4/12 px-4">
        <div
          class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-blueGray-200 border-0"
        >
          <div class="rounded-t mb-0 px-6 py-6">
            <div class="text-center mb-3">
              <h6 class="text-blueGray-500 text-lg font-bold">
                WRONG URL
              </h6>
            </div>
            <hr class="mt-6 border-b-1 border-blueGray-300" />
          </div></div></div></div></div>

  </div>
</template>


<script>
import firebase from "../../firebase";
import VueCookies from 'vue-cookies';
import CryptoJS from "crypto-js";
export default {
  layout: "LoginLayout",
  data() {
    return {
      companyData: null,
      candidateData: null,
      eligible: null,
      loading: true,
      permissions: {video: false, audio: false},
      form: {
        email: "",
        languages: [],
      },
    };
  },
  methods: {
    async grantPermissions (value) {
      if (value === 'audio'){
          try {
            const req = await navigator.mediaDevices.getUserMedia({ audio: true});
            if (req) this.permissions.audio = true
          } catch (err) {
            alert(err)
          }
        } else {
          try {
            const req = await navigator.mediaDevices.getUserMedia({ video: true});
            if (req) this.permissions.video = true
          } catch (err) {
            alert(err)
          }
        }
    },
    async startTest() {
      if (this.permissions.video && this.permissions.audio) {
      try {
      var cipherEmail = CryptoJS.AES.encrypt(this.candidateData.email,"736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString();
      var cipherUid = CryptoJS.AES.encrypt(this.$route.params.id,"736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString();
      VueCookies.set('fbb3cu24', cipherUid);
      VueCookies.set('fbb3em24', cipherEmail);
      this.loading = true
      const entry = await firebase
        .firestore()
        .collection("accounts")
        .doc(this.$route.params.id);
      const details = this.candidateData
      await entry.update({
        "candidates.invited": firebase.firestore.FieldValue.arrayRemove(this.candidateData),
      });
      details.score = 0
      details.timeStamp = String(new Date())
      await entry.update({
        "candidates.ongoing": firebase.firestore.FieldValue.arrayUnion(details),
      });
      this.$router.push({
        path: "/admin/ticket",
      });
      } catch (err) {
        alert(err.message)
      }
      } else{
        alert ("Need video and audio permissions")
      }
    },
    async fetchFirestore() {
      try{
      const snapshot = await firebase
        .firestore()
        .collection("accounts")
        .doc(this.$route.params.id)
        .get();
      if(snapshot.data()) {
        this.companyExists = true
        this.companyData = snapshot.data();
      }else {
        this.companyExists = null
      }
      }
      catch(err) {
        alert(err.message)
      }
      this.loading = null
    },
    signup() {
      this.loading = true;
      const applicants = this.companyData.candidates.invited;
      applicants.forEach((e) => {
        if (e.email == this.form.email) {
          this.eligible = true;
          this.candidateData = e;
        }
      });
      if (!this.eligible) {
        alert("Not Invited");
        this.loading = null;
      } else {
        this.form.languages = Object.values(this.candidateData.tags);
        this.loading = null;
      }
    },
  },
  created() {
    this.fetchFirestore();
  },
};
</script>