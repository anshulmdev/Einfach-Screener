<template>
<div>
  <div
    class="relative flex flex-col min-w-0 break-words bg-white w-full mb-6 shadow-lg rounded"
  >
    <div class="block w-full overflow-x-auto">
      <!-- Projects table -->
      <table
        v-if="response && !response.err"
        class="items-center w-full bg-transparent border-collapse"
        style="max-height: 150px min-height: 150px"
      >
        <thead class="thead-light">
          <tr>
            <th
              class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left"
            >
              Test Cases
            </th>
            <th
              class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left"
            >
              Time Used
            </th>
            <th
              class="px-6 bg-blueGray-50 text-blueGray-500 align-middle border border-solid border-blueGray-100 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-nowrap font-semibold text-left min-w-140-px"
            >
              Passed
            </th>
          </tr>
        </thead>
        <tbody v-if="response[0].output">
          <tr v-for="(item, index) in testCases" :key="index">
            <th
              class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4 text-left"
            >
              Test Case {{ index }}
            </th>
            <td
              class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4"
            >
              {{ item.time }} ms
            </td>
            <td
              class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4"
            >
              <div class="flex items-center">
                <span class="mr-2">{{ item.output === true ? 100 : 0 }}%</span>
                <div class="relative w-full">
                  <div
                    class="overflow-hidden h-2 text-xs flex rounded bg-red-200"
                  >
                    <div
                      :style="`width: ${item.output === true ? 100 : 0}%;`"
                      class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-emerald-500"
                    ></div>
                  </div>
                </div>
              </div>
            </td>
          </tr>
        </tbody>
        <tbody v-else>
            <tr v-for="(item, index) in [1,2,3]" :key="index">
            <th
              class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4 text-left"
            >
              Test Case {{index}}
            </th>
            <td
              class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4"
            >
              0 ms
            </td>
            <td
              class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-nowrap p-4"
            >
              <span class="text-xs font-semibold inline-block py-1 px-2 uppercase rounded text-red-600 bg-red-200 uppercase last:mr-0 mr-1">
                {{status}}
              </span>
            </td>
          </tr>
        </tbody>
      </table>

      <div v-else class="console">
        {{ response.err }}
      </div>
    </div>
  </div>
        <div class="rounded-t mb-0 px-4 py-3 border-0">
        <div class="flex flex-wrap items-center">
          <div class="relative w-full px-4 max-w-full flex-grow flex-1">
            
          </div>
          <ul
            class="flex-col md:flex-row list-none items-center hidden md:flex"
          >
            <button
              class="bg-emerald-600 text-white active:bg-emerald-700  font-bold uppercase text-xs px-4 py-2 rounded shadow hover:shadow-md outline-none focus:outline-none ease-linear transition-all duration-150"
              type="button"
              @click="finalSubmit"
            >
              Submit Score
            </button>
          </ul>
        </div>
      </div>
  </div>
</template>

<script>
import VueCookies from "vue-cookies";
import firebase from "../../firebase";
import CryptoJS from "crypto-js";
export default {
  props: {
    response: Object,
    marks: Number,
    question: String,
    code: String,
    description: String,
    status: String
  },
  data () {
    return {
      testCases: this.response.slice(0,3),
    }
  },
  watch: {
    response () {
      this.testCases = this.response.slice(0,3)
    }
  },
  methods: {
    // eslint-disable-next-line no-unused-vars
    async submit(score) {
      const details = {}
      details[CryptoJS.AES.decrypt(VueCookies.get("fbb3em24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)] = {}
      const req = await firebase.firestore().collection("scores").doc(CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8))
      const res = await req.get()
      const response = res.data()[CryptoJS.AES.decrypt(VueCookies.get("fbb3em24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)]
      if (response.coding) {
        response.coding[this.question] = {score, question: this.question, code: this.code, description: this.description}
      } else{
        response.coding = {}
        response.coding[this.question] = {score, question: this.question, code: this.code, description: this.description}
      }
      details[CryptoJS.AES.decrypt(VueCookies.get("fbb3em24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)] = response
      await firebase.firestore().collection("scores").doc(CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)).set(details, {merge: true});
    },
    finalSubmit () {
      if (this.response && !this.response.err) {
        const answers = Object.values(this.response).filter((e) => e.output === true)
        const percentage = answers.length / Object.values(this.response).length
        const score = parseInt(percentage * this.marks)
        this.submit(score)
      } else{
        this.submit(0)
      }
    }
  },
};
</script>

<style>
.console {
  background: #2d2d2d;
  color: #ccc;
  font-family: Fira code, Fira Mono, Consolas, Menlo, Courier, monospace;
  font-size: 14px;
  line-height: 1.5;
  padding: 4px;
  min-height: 150px;
  max-height: 150px;
}
</style>