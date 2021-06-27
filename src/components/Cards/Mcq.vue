<template>
  <div>
    <div
      v-if="questions"
      class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded"
      :class="[color === 'light' ? 'bg-white' : 'bg-emerald-900 text-white']"
    >
      <div class="rounded-t mb-0 px-4 py-3 border-0">
        <div class="flex flex-wrap items-center">
          <div class="relative w-full px-4 max-w-full flex-grow flex-1">
            <h3
              class="font-semibold text-lg"
              :class="[color === 'light' ? 'text-blueGray-700' : 'text-white']"
            >
              {{currentQuestion}}
            </h3>
          </div>
        </div>
      </div>
      <div class="block w-full overflow-x-auto">
        <!-- Projects table -->
        <table class="items-center w-full bg-transparent border-collapse">
          <thead>
            <tr>
              <th
                class="px-6 align-middle border border-solid py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left"
                :class="[
                  color === 'light'
                    ? 'bg-blueGray-50 text-blueGray-500 border-blueGray-100'
                    : 'bg-emerald-800 text-emerald-300 border-emerald-700',
                ]"
              >
                Question
              </th>
              <th
                class="px-6 align-middle border border-solid py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left"
                :class="[
                  color === 'light'
                    ? 'bg-blueGray-50 text-blueGray-500 border-blueGray-100'
                    : 'bg-emerald-800 text-emerald-300 border-emerald-700',
                ]"
              >
                Marks
              </th>
              <th
                class="px-6 align-middle border border-solid py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left"
                :class="[
                  color === 'light'
                    ? 'bg-blueGray-50 text-blueGray-500 border-blueGray-100'
                    : 'bg-emerald-800 text-emerald-300 border-emerald-700',
                ]"
              >
                Completion
              </th>
              <th
                class="px-6 align-right border border-solid py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-right"
                :class="[
                  color === 'light'
                    ? 'bg-blueGray-50 text-blueGray-500 border-blueGray-100'
                    : 'bg-emerald-800 text-emerald-300 border-emerald-700',
                ]"
              >
                Select Correct Answer
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in questions" :key="index">
              <th
                class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4 text-left flex items-center"
              >
                <img
                  src="https://static.vecteezy.com/system/resources/thumbnails/002/003/610/small/question-marks-flat-color-illustration-vector.jpg"
                  class="h-12 w-12 bg-white rounded-full border"
                  alt="..."
                />
                <button
                @click="currentQuestion = item.heading"
                  class="ml-3 font-bold"
                  :class="[
                    color === 'light' ? 'text-blueGray-600' : 'text-white',
                  ]"
                >
                  {{ heading(item.heading) }}
                </button>
              </th>
              <td
                class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4"
              >
                {{ item.marks }} marks
              </td>
              <td
                class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4"
              >
                <div class="flex items-center">
                  <span class="mr-2">{{ item.selected ? 100 : 0 }} %</span>
                  <div class="relative w-full">
                    <div
                      class="overflow-hidden h-2 text-xs flex rounded bg-red-200"
                    >
                      <div
                        :style="`width: ${item.selected ? 100 : 0}%;`"
                        class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-emerald-500"
                      ></div>
                    </div>
                  </div>
                </div>
              </td>
              <td
                class="border-t-0 px-8 align-right border-l-0 border-r-0 text-xs whitespace-nowrap p-4 text-right"
              >
                <select
                  v-model="questions[index].selected"
                  class="border border-gray-300 rounded-sm text-gray-600 h-10 pl-3 pr-8 bg-white hover:border-gray-400 focus:outline-none appearance-none"
                >
                  <option value="">Choose Answer</option>
                  <option v-for="(option, index) in item.options" :key="index">
                    {{ option }}
                  </option>
                </select>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div
      v-if="questions"
      class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded"
      :class="[color === 'light' ? 'bg-white' : 'bg-emerald-900 text-white']"
    >
      <div class="rounded-t mb-0 px-4 py-3 border-0">
        <div class="flex flex-wrap items-center">
          <div class="relative w-full px-4 max-w-full flex-grow flex-1">
            <button
              class="bg-emerald-500 text-white active:bg-emerald-600 font-bold uppercase text-xs px-4 py-2 rounded shadow hover:shadow-md outline-none focus:outline-none mr-1 ease-linear transition-all duration-150"
              type="button"
              @click="submit"
            >
              Submit Answers
            </button>
          </div>
        </div>
      </div>
    </div>
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
  </div>
</template>
<script>
import VueCookies from "vue-cookies";
import firebase from "../../firebase";
import CryptoJS from "crypto-js";

export default {
  data() {
    return {
      questions: null,
      loading : null,
      currentQuestion: 'Multiple Choice Questions'
    };
  },
  props: {
    color: {
      default: "light",
      validator: function (value) {
        // The value must match one of these strings
        return ["light", "dark"].indexOf(value) !== -1;
      },
    },
  },
  methods: {
    heading (value) {
      if (value.length <=50) {
        return value
      } else{
        return `${value.slice(0,20)} - [click to view]`
      }
    },
    async submit() {
      const database = this.questions;
      const req = await fetch(
        "https://hire-298805-default-rtdb.firebaseio.com/mcqAnswers.json",
        {
          method: "post",
          body: JSON.stringify(database),
          headers: { "Content-Type": "application/json; charset=UTF-8" },
        }
      );
      const score = await req.json();
      const attempted = Object.values(this.questions).filter((e) => e.selected).length
      const details = {}
      details[CryptoJS.AES.decrypt(VueCookies.get("fbb3em24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)] = {mcq: {score, attempted}}
      await firebase.firestore().collection("scores").doc(CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)).set(details, {merge: true});
    },
    async fetchRTDB() {
      if (CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8) && CryptoJS.AES.decrypt(VueCookies.get("fbb3em24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)) {
        const companyUid = CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8);
        const snapshotData = await firebase
          .firestore()
          .collection("accounts")
          .doc(companyUid)
          .get();
        const mcqIndex = snapshotData.data().assignment.tags.mcq;
        const req = await firebase.database().ref("mcq").get();
        const mcqDatabase = await req.val();
        const finalSet = {};
        mcqIndex.forEach((e) => {
          finalSet[e.index] = mcqDatabase[e.index];
          finalSet[e.index].selected = "";
        });
        this.questions = finalSet;
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
    this.fetchRTDB();
  },
};
</script>

