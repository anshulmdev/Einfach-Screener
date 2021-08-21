<template>
  <div>
    <div v-if="questions" class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded" :class="[color === 'light' ? 'bg-white' : 'bg-emerald-900 text-white']">
      <div class="rounded-t mb-0 px-4 py-3 border-0">
        <div class="flex flex-wrap items-center">
          <div class="w-full mx-autp items-center flex justify-between md:flex-nowrap flex-wrap md:pr-10 px-4">
            <h3 class="font-semibold text-lg" :class="[color === 'light' ? 'text-black-700' : 'text-white']">
              Questions: {{ currentQuestionIndex + 1 }} :
              {{ Object.values(questions)[currentQuestionIndex].heading }}
            </h3>
            <ul class="flex-col md:flex-row list-none items-center hidden md:flex pt-2">
              <div v-if="languages">
                <select v-model="languageSelected" class="py-1 px-1 rounded-md mt-1 text-black">
                  <option v-for="item in languages" :key="item">{{ item.name }}</option>
                </select>
              </div>
            </ul>
          </div>
        </div>
      </div>
      <div class="flex content-center flex-wrap mx-2 p-3 bg-grey rounded shadow-lg">
        <div class="flex w-1/2 px-2 py-2">
          <div class="p-4 rounded shadow-lg bg-white text-black border-b border-r border-grey-dark">
            <div class="customHeight overflow-auto" v-html="Object.values(questions)[currentQuestionIndex].description" id="app" />
          </div>
        </div>
        <div class="flex-grow w-1/2 px-2 py-2">
          <div class="p-4 rounded shadow-lg bg-white text-black border-b border-r border-grey-dark">
            <prism-editor class="my-editor" v-model="assignTemplate[checkQues]" :highlight="highlighter" line-numbers language="js"></prism-editor>
          </div>
          <div class="pt-2">
            <div class="p-4 rounded shadow-lg bg-white text-black border-b border-r border-grey-dark">
              <div class="flex flex-wrap">
                <div class="w-full">
                  <ul class="flex mb-0 list-none flex-wrap pt-3 pb-4 flex-row">
                    <li class="-mb-px mr-2 last:mr-0 flex-auto text-center">
                      <a
                        class="text-xs font-bold uppercase px-5 py-3 shadow-lg rounded block leading-normal cursor-pointer"
                        v-on:click="toggleTabs(1)"
                        v-bind:class="{
                          'text-pink-600 bg-white': openTab !== 1,
                          'text-white bg-emerald-900': openTab === 1,
                        }"
                      >
                        Test Cases
                      </a>
                    </li>
                    <li class="-mb-px mr-2 last:mr-0 flex-auto text-center">
                      <a
                        class="text-xs font-bold uppercase px-5 py-3 shadow-lg rounded block leading-normal cursor-pointer"
                        v-on:click="toggleTabs(2)"
                        v-bind:class="{
                          'text-pink-600 bg-white': openTab !== 2,
                          'text-white bg-emerald-900': openTab === 2,
                        }"
                      >
                        Test and Run
                      </a>
                    </li>
                    <li class="-mb-px mr-2 last:mr-0 flex-auto text-center">
                      <a
                        class="text-xs font-bold uppercase px-5 py-3 shadow-lg rounded block leading-normal cursor-pointer"
                        v-on:click="toggleTabs(3)"
                        v-bind:class="{
                          'text-pink-600 bg-white': openTab !== 3,
                          'text-white bg-emerald-900': openTab === 3,
                        }"
                      >
                        Submit
                      </a>
                    </li>
                  </ul>
                  <div class="relative flex flex-col min-w-0 break-words bg-white w-full mb-6 shadow-lg rounded">
                    <div class="px-4 py-5 flex-auto">
                      <div class="tab-content tab-space">
                        <div
                          v-bind:class="{
                            hidden: openTab !== 1,
                            block: openTab === 1,
                          }"
                        >
                          <textarea v-model="this.testTemplate[this.checkQues]" type="text" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" rows="6" />
                        </div>
                        <div
                          v-bind:class="{
                            hidden: openTab !== 2,
                            block: openTab === 2,
                          }"
                        >
                          <textarea v-model="runResponse" type="text" class="console w-full" rows="6" disabled />
                        </div>
                        <div
                          v-bind:class="{
                            hidden: openTab !== 3,
                            block: openTab === 3,
                          }"
                        >
                          <CodeSubmit v-if="submitResponse" :status="status" :question="Object.values(questions)[currentQuestionIndex].heading" :description="Object.values(questions)[currentQuestionIndex].description" :code="this.assignTemplate[this.checkQues]" :response="submitResponse" :marks="parseInt(Object.values(questions)[currentQuestionIndex].marks)" />
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded" :class="[color === 'light' ? 'bg-white' : 'bg-emerald-900 text-white']">
      <div class="rounded-t mb-0 px-4 py-3 border-0">
        <div class="flex flex-wrap items-center">
          <div class="relative w-full px-4 max-w-full flex-grow flex-1">
            <button class="bg-emerald-500 text-white active:bg-emerald-600 font-bold uppercase text-xs px-4 py-2 rounded shadow hover:shadow-md outline-none focus:outline-none mr-1 ease-linear transition-all duration-150" type="button" @click="back">Previous</button>
          </div>
          <ul class="flex-col md:flex-row list-none items-center hidden md:flex">
            <button class="bg-emerald-600 text-white active:bg-emerald-800 font-bold uppercase text-xs px-4 py-2 rounded shadow hover:shadow-md outline-none focus:outline-none mr-1 ease-linear transition-all duration-150" type="button" @click="next">Next</button>
          </ul>
        </div>
      </div>
    </div>
    <div v-if="loading">
      <link rel="stylesheet" href="https://pagecdn.io/lib/font-awesome/5.10.0-11/css/all.min.css" integrity="sha256-p9TTWD+813MlLaxMXMbTA7wN/ArzGyW/L7c5+KkjOkM=" crossorigin="anonymous" />

      <div class="w-full h-full fixed block top-0 left-0 bg-white opacity-75 z-50">
        <span class="text-green-500 opacity-75 top-1/2 my-0 mx-auto block relative w-0 h-0" style="top: 50%; left: 50%">
          <i class="fas fa-circle-notch fa-spin fa-5x"></i>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import CodeSubmit from "@/components/Assignment/CodeSubmit.vue"
import { PrismEditor } from "vue-prism-editor"
import "vue-prism-editor/dist/prismeditor.min.css" // import the styles somewhere

// import highlighting library (you can use any library you want just return html string)
import { highlight, languages } from "prismjs/components/prism-core"
import "prismjs/components/prism-clike"
import "prismjs/components/prism-javascript"
import "prismjs/themes/prism-tomorrow.css" // import syntax highlighting styles

import VueCookies from "vue-cookies"
import firebase from "../../firebase"
import CryptoJS from "crypto-js"
import axios from "axios"

export default {
  data() {
    return {
      submitResponse: [{ time: 0, output: null }],
      currentQuestionIndex: 0,
      totalQuestion: 0,
      runResponse: `Loading...`,
      color: "dark",
      questions: null,
      openTab: 1,
      loading: null,
      status: 'Loading...',
      assignTemplate: {
        [`${this.category}063`]: `process.stdin.once('data', (chunk) => { 

yourInput = chunk.toString()

// WRITE YOUR CODE HERE

var yourFunction = function(nums) {
    nums = JSON.parse(nums)
    return nums
};
console.log(yourFunction(yourInput ))

} )`,
      },
      testTemplate: { [`${this.category}063`]: "[1, 2, 3, 4]" },
      code: {
        71: `testCase = input().strip('][').split(',')

def yourFunction (nums):
    return nums
    
    
print(yourFunction(testCase))`,
        63: `process.stdin.once('data', (chunk) => { 

yourInput = chunk.toString()

// WRITE YOUR CODE HERE

var yourFunction = function(nums) {
    nums = JSON.parse(nums)
    return nums
};
console.log(yourFunction(yourInput ))

} )`,
      },
      test: {
        71: "[1,2,3,4]",
        63: "[5,6,7,8]",
      },
      languages: [{id: 63, name: "JavaScript (Node.js 12.14.0)"}, {id: 71, name: "Python (3.8.1)"}],
      languageSelected: "JavaScript (Node.js 12.14.0)",
    }
  },
  props: {
    category: String,
  },
  watch: {
    category() {
      this.checkAndFetch()
      if (!this.assignTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`]) this.assignTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`] = this.code[this.checkId]
      if (!this.testTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`]) this.testTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`] = this.test[this.checkId]
    },
    languageSelected() {
      if (!this.assignTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`]) this.assignTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`] = this.code[this.checkId]
      if (!this.testTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`]) this.testTemplate[`${this.category}${this.currentQuestionIndex}${this.checkId}`] = this.test[this.checkId]
    },
  },
  methods: {
    // eslint-disable-next-line no-unused-vars
    async submitCode(code, question, category, id) {
      const response = await axios.post('https://einfach.api.stdlib.com/application@dev/code/codeSubmit/', {code, question, category, id})
      this.submitResponse = response.data
      this.loading = null
      setTimeout(() => {
        this.status = 'Failed'
      }, 10000);
    },
    async runSingleCode() {
      try {
      const body = {code: this.assignTemplate[this.checkQues], testCase: this.testTemplate[this.checkQues], id: this.checkId}
      const req = await axios.post('https://einfach.api.stdlib.com/application@dev/code/codeCheck/', body)
      this.runResponse = req.data
      } catch(err) {
        this.runResponse = err.message
      }
    },
    async checkAndFetch() {
      this.openTab = 1
      this.currentQuestionIndex = 0
      this.totalQuestion = 1
      const companyUid = CryptoJS.AES.decrypt(VueCookies.get("fbb3cu24"), "736b9960-fbb3-4430-a653-f9f4d58ddfe1").toString(CryptoJS.enc.Utf8)
      const snapshotData = await firebase.firestore().collection("accounts").doc(companyUid).get()
      const req = snapshotData.data().assignment.tags
      if (req[this.category]) {
        const codingIndex = snapshotData.data().assignment.tags[this.category]
        const reqDatabase = await firebase.database().ref(`coding/${this.category}`).get()
        const codeDatabase = await reqDatabase.val()
        const finalSet = {}
        codingIndex.forEach((e) => {
          finalSet[e.index] = codeDatabase[e.index]
          finalSet[e.index].selected = ""
        })
        this.questions = finalSet
        this.totalQuestion = Object.keys(finalSet).length
      } else {
        this.$router.push({ path: "/" })
      }
      this.loading = null
    },
    toggleTabs: function (tabNumber) {
      this.submitResponse = [{ time: 0, output: null }]
      this.status = 'Loading...'
      this.runResponse = `Loading...`
      this.openTab = tabNumber
      if (tabNumber === 2) {
        this.loading = true
        this.runSingleCode()
        this.loading = null
      }
      if (tabNumber === 3) {
        this.loading = true
        this.submitCode(this.assignTemplate[this.checkQues], Object.values(this.questions)[this.currentQuestionIndex].heading, this.category, this.checkId)
      }
    },
    highlighter(code) {
      return highlight(code, languages.js) // languages.<insert language> to return html with markup
    },
    next() {
      if (this.currentQuestionIndex < this.totalQuestion) {
        this.openTab = 1
        this.currentQuestionIndex += 1
      }
    },
    back() {
      if (this.currentQuestionIndex > 0) {
        this.openTab = 1
        this.currentQuestionIndex -= 1
      }
    },
  },
  components: {
    PrismEditor,
    CodeSubmit,
  },
  computed: {
    checkId() {
      if (this.languages) return this.languages.filter((e) => e.name === this.languageSelected)[0].id
      else return 63
    },
    checkQues() {
      return `${this.category}${this.currentQuestionIndex}${this.checkId}`
    },
  },
  created() {
    this.loading = true
    this.checkAndFetch()
  },
}
</script>

<style>
/* required class */
.my-editor {
  /* you must provide font-family font-size line-height. Example: */
  font-family: Fira code, Fira Mono, Consolas, Menlo, Courier, monospace;
  font-size: 14px;
  line-height: 1.5;
  padding: 2px;
  min-height: 420px;
  max-height: 420px;
}
.customHeight {
  min-height: 730px;
  max-height: 730px;
}

/* optional class for removing the outline */
.prism-editor__textarea:focus {
  outline: none;
}
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
