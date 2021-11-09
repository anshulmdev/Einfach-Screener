<template>
  <div>
    <div v-if="formData" class="container mx-auto px-4 h-full">
      <div class="flex content-center items-center justify-center h-full">
        <div class="px-4 w-10/12">
          <div class="p-4 relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-blueGray-200 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent border-2 border-purple-300">
            <div class="rounded-t mb-0 px-4 py-4">
              <div class="text-center mb-3 flex">
                <img alt="..." class="w-16 mr-2 rounded-full flex content-start items-start justify-start" :src="formData.logo" />
                <h6 class="text-blueGray-900 text-2xl font-bold ml-4 pl-4 flex content-center items-center justify-center">
                  {{ formData.title }}
                </h6>
              </div>
            </div>
            <div class="flex-auto px-4 pt-0">
              <div class="mt-1 py-5 border-t border-blueGray-200 text-center">
                <div class="flex flex-wrap justify-center">
                  <div class="w-full px-4">
                    <p class="mb-1 text-lg leading-relaxed text-blueGray-700">
                      {{ formData.desc }}
                    </p>
                  </div>
                </div>
              </div>

              <div v-if="showSuccessPopup" class="flex w-full max-w-sm mx-auto overflow-hidden bg-white rounded-lg shadow-md dark:bg-gray-800 m-4">
                <div class="flex items-center justify-center w-12 bg-emerald-500">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 m-2" fill="white" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                </div>

                <div class="px-4 py-2 -mx-3">
                  <div class="mx-3">
                    <span class="font-semibold text-emerald-500 dark:text-green-400">Success</span>
                    <p class="text-sm text-gray-600 dark:text-gray-200">Your form has been submitted successfully</p>
                  </div>
                </div>
              </div>
              <hr class="mt-1 mb-6 pb-6 border-b-1 border-blueGray-300" />
              <form>
                <div class="grid grid-cols-1 mb-2 md:grid-cols-2 gap-5 md:gap-8 mt-5">
                  <div class="grid grid-cols-1 mr-2">
                    <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" htmlFor="grid-password"> First Name </label>
                    <input v-model="form.fname" type="email" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" placeholder="John" />
                  </div>
                  <div class="grid grid-cols-1 ml-2">
                    <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" htmlFor="grid-password"> Last Name </label>
                    <input v-model="form.lname" type="email" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" placeholder="Doe" />
                  </div>
                </div>
                <div class="grid grid-cols-1 mb-2 md:grid-cols-2 gap-5 md:gap-8 mt-5">
                  <div class="grid grid-cols-1 mr-2">
                    <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" htmlFor="grid-password"> Email </label>
                    <input v-model="form.email" type="email" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" placeholder="jd57@gmail.com" />
                  </div>
                  <div class="grid grid-cols-1 ml-2">
                    <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" htmlFor="grid-phone"> Phone </label>
                    <input v-model="form.phone" type="number" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" placeholder="+91 7812983311" />
                  </div>
                </div>

                <div class="grid grid-cols-1 mb-2 md:grid-cols-2 gap-5 md:gap-8 mt-5">
                  <div class="grid grid-cols-1 mr-2">
                    <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" htmlFor="grid-password"> Area of Expertise </label>
                    <select v-model="form.area" class="py-2 px-3 rounded-md mt-1">
                      <option>JavaScript</option>
                      <option>Node Js</option>
                      <option>Python</option>
                      <option>Digital Ocean</option>
                      <option>Azure</option>
                      <option>Amazon Web</option>
                      <option>Google Cloud</option>
                      <option>Fresher</option>
                    </select>
                  </div>
                  <div class="grid grid-cols-1 ml-2">
                    <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" htmlFor="grid-password"> Experience </label>
                    <input v-model="form.exp" type="number" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" placeholder="5 Years" />
                  </div>
                </div>

                <div class="grid grid-cols-1 mt-5 mx-7">
                  <label class="block uppercase text-blueGray-600 text-xs font-bold mt-4" htmlFor="grid-password"> Upload Resume </label>
                  <div v-if="!form.resume" class="flex items-center justify-center w-full shadow-sm mt-5 rounded-md bg-white cursor-pointer">
                    <label class="flex flex-col border-4 border-dashed w-full h-32 hover:bg-gray-100 hover:border-purple-300 group">
                      <div class="flex flex-col items-center justify-center py-5">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7v8a2 2 0 002 2h6M8 7V5a2 2 0 012-2h4.586a1 1 0 01.707.293l4.414 4.414a1 1 0 01.293.707V15a2 2 0 01-2 2h-2M8 7H6a2 2 0 00-2 2v10a2 2 0 002 2h8a2 2 0 002-2v-2" />
                        </svg>
                        <p class="text-sm text-gray-400 group-hover:text-purple-600 pt-1 tracking-wider">Upload PDF Only</p>
                      </div>
                      <input type="file" id="file" ref="file" accept=".pdf" v-on:change="handleFileUpload(true)" class="hidden" />
                    </label>
                  </div>
                  <div v-else class="flex items-center justify-center w-full shadow-sm mt-5 rounded-md bg-white cursor-pointer">
                    <label class="flex flex-col border-4 border-dashed w-full h-32 hover:bg-gray-100 hover:border-purple-300 group">
                      <div class="flex flex-col items-center justify-center py-5">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4" />
                        </svg>
                        <p class="text-sm text-gray-400 group-hover:text-purple-600 pt-1 tracking-wider">Resume is added</p>
                        <button @click="form.resume = null" class="border border-red-500 bg-red-500 text-white rounded-md px-4 py-2 m-2 transition duration-500 ease select-none hover:bg-red-600 focus:outline-none focus:shadow-outline">
                          Remove
                        </button>
                      </div>
                    </label>
                  </div>
                </div>
                <div class="text-center mt-6 pt-6 mb-6">
                  <button @click="apply()" class="bg-blueGray-800 text-white active:bg-blueGray-600 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full ease-linear transition-all duration-150" type="button">Apply Position</button>
                </div>
              </form>
            </div>
          </div>
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
import firebase from "../../firebase"
import { DB } from "../../firebase"
export default {
  layout: "LoginLayout",
  data() {
    return {
      formData: null,
      loading: true,
      showSuccessPopup: null,
      submit: true,
      form: {
        fname: null,
        lname: null,
        email: null,
        phone: null,
        area: null,
        exp: null,
        resume: null,
      },
    }
  },
  methods: {
    async apply() {
      this.loading = true
      if (this.submit && this.form.fname && this.form.lname && this.form.email && this.form.phone && this.form.area && this.form.exp && this.form.resume) {
        const { email, exp: experience, fname, lname, phone, area } = this.form
        const areaLabel = { JavaScript:"warning", "Node Js": "success", Python: "success", "Digital Ocean": "primary", Azure: "primary", "Amazon Web": "danger", "Google Cloud": "info", Fresher: "warning" }
        var storageRef = firebase.storage().ref()
        var imageRef = storageRef.child(`${this.$route.params.id}/resume/${this.form.email}`)
        // eslint-disable-next-line no-unused-vars
        const url = await imageRef.put(this.form.resume)
        var resume = await imageRef.getDownloadURL()
        const details = { email, experience, name: `${fname} ${lname}`, phone, resume, tags: { [areaLabel[area]]: area }, time: String(new Date()) }
        const actualPath = await firebase.firestore().collection("accounts").doc(this.$route.params.id)
        // eslint-disable-next-line no-unused-vars
        const res = await actualPath.update({ "candidates.applied": firebase.firestore.FieldValue.arrayUnion(details) })
        this.showSuccessPopup = true
        this.submit = false
        setTimeout(() => {
          this.showSuccessPopup = null
        }, 5000)
      } else alert("Data Missing or Submitted Already")
      this.loading = null
    },
    handleFileUpload(func) {
      if (func === true) this.form.resume = this.$refs.file.files[0]
      else this.form.resume = null
    },
    async fetchRtdb() {
      try {
        const data = await DB.ref(`forms/${this.$route.params.id}`)
        data
          .get()
          .then((snapshot) => {
            if (snapshot.exists()) {
              this.formData = snapshot.val()
            } else {
              console.log("No data available")
            }
          })
          .catch((error) => {
            console.error(error)
          })
      } catch (err) {
        alert(err.message)
      }
      this.loading = null
    },
  },
  created() {
    this.fetchRtdb()
  },
}
</script>
