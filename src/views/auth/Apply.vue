<template>
  <div>
    <div v-if="formData" class="container mx-auto px-4 h-full">
      <div class="flex content-center items-center justify-center h-full">
        <div class="w-8/12 px-4">
          <div
            class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-blueGray-200  focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent  border-2 border-purple-300"
          >
        
            <div class="rounded-t mb-0 px-4 py-4">
              <div
                class="text-center mb-3 flex"
              >
                <img
                  alt="..."
                  class="w-16 mr-2 rounded-full flex content-start items-start justify-start"
                  :src="formData.logo"
                />
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
              <hr class="mt-1 mb-6 pb-6 border-b-1 border-blueGray-300" />
              <form>
                <div
                  class="grid grid-cols-1 mb-2  md:grid-cols-2 gap-5 md:gap-8 mt-5 "
                >
                  <div class="grid grid-cols-1 mr-2">
                    <label
                      class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                      htmlFor="grid-password"
                    >
                      First Name
                    </label>
                    <input
                      v-model="form.fname"
                      type="email"
                      class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                      placeholder="John"
                    />
                  </div>
                  <div class="grid grid-cols-1 ml-2">
                    <label
                      class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                      htmlFor="grid-password"
                    >
                      Last Name
                    </label>
                    <input
                      v-model="form.lname"
                      type="email"
                      class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                      placeholder="Doe"
                    />
                  </div>
                </div>
                <div
                  class="grid grid-cols-1 mb-2  md:grid-cols-2 gap-5 md:gap-8 mt-5 "
                >
                  <div class="grid grid-cols-1 mr-2">
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
                      placeholder="jd57@gmail.com"
                    />
                  </div>
                  <div class="grid grid-cols-1 ml-2">
                    <label
                      class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                      htmlFor="grid-phone"
                    >
                      Phone
                    </label>
                    <input
                      v-model="form.phone"
                      type="number"
                      class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                      placeholder="+91 7812983311"
                    />
                  </div>
                </div>

                <div
                  class="grid grid-cols-1 mb-2  md:grid-cols-2 gap-5 md:gap-8 mt-5 "
                >
                  <div class="grid grid-cols-1 mr-2">
                    <label
                      class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                      htmlFor="grid-password"
                    >
                      Area of Expertise
                    </label>
                    <select v-model="form.area" class="py-2 px-3 rounded-md mt-1">
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
                    <label
                      class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                      htmlFor="grid-password"
                    >
                      Experience
                    </label>
                    <input
                    v-model="form.exp"
                      type="number"
                      class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150"
                      placeholder="5 Years"
                    />
                  </div>
                </div>

                <div class="grid grid-cols-1 mt-5 mx-7">
                  <label
                    class="block uppercase text-blueGray-600 text-xs font-bold mb-4"
                    htmlFor="grid-password"
                  >
                    Upload Resume
                  </label>
                  <div
                    class="flex items-center justify-center w-full shadow-sm mt-5 rounded-md bg-white cursor-pointer"
                  >
                    <label
                      class="flex flex-col border-4 border-dashed w-full h-32 hover:bg-gray-100 hover:border-purple-300 group"
                    >
                      <div
                        class="flex flex-col items-center justify-center py-5"
                      >
                        <svg
                          class="w-10 h-10 text-purple-400 group-hover:text-purple-600"
                          fill="none"
                          stroke="currentColor"
                          viewBox="0 0 24 24"
                          xmlns="http://www.w3.org/2000/svg"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
                          ></path>
                        </svg>
                        <p
                          class="lowercase text-sm text-gray-400 group-hover:text-purple-600 pt-1 tracking-wider"
                        >
                          Select [Pdf, Doc, Image]
                        </p>
                      </div>
                      <input type="file" id="file" ref="file" v-on:change="handleFileUpload(true)" class="hidden" />
                    </label>
                  </div>
                </div>
                <div class="text-center mt-6 pt-6 mb-6">
                  <button
                  @click="apply()"
                    class="bg-blueGray-800 text-white active:bg-blueGray-600 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full ease-linear transition-all duration-150"
                    type="button"
                  >
                    Apply Position
                  </button>
                </div>
              </form>
              
          
    <div v-if="showSuccessPopup" class="flex w-full max-w-sm mx-auto overflow-hidden bg-white rounded-lg shadow-md dark:bg-gray-800 m-4">
        <div class="flex items-center justify-center w-12 bg-emerald-500">
            <svg class="w-6 h-6 text-white fill-current" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
                <path d="M20 3.33331C10.8 3.33331 3.33337 10.8 3.33337 20C3.33337 29.2 10.8 36.6666 20 36.6666C29.2 36.6666 36.6667 29.2 36.6667 20C36.6667 10.8 29.2 3.33331 20 3.33331ZM16.6667 28.3333L8.33337 20L10.6834 17.65L16.6667 23.6166L29.3167 10.9666L31.6667 13.3333L16.6667 28.3333Z"/>
            </svg>
        </div>
        
        <div class="px-4 py-2 -mx-3">
            <div class="mx-3">
                <span class="font-semibold text-emerald-500 dark:text-green-400">Success</span>
                <p class="text-sm text-gray-600 dark:text-gray-200">Your form has been submitted successfully</p>
            </div>
        </div>
    </div>
            </div>
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
import firebase from '../../firebase';
import { DB } from "../../firebase";
export default {
  layout: "LoginLayout",
  data() {
    return {
      formData: null,
      loading: true,
      showSuccessPopup: null,
      form: {
        fname: null,
        lname: null,
        email: null,
        phone: null,
        area: null,
        exp: null,
        resume: null
      }
    };
  },
  methods: {
    async apply() {
      this.loading = true;
      if(this.form.fname && this.form.lname && this.form.email && this.form.phone && this.form.area && this.form.exp && this.form.resume){
        const {email, exp: experience, fname, lname, phone, area} = this.form
        const areaLabel = {"Node Js": "success","Python": "success","Digital Ocean": "primary","Azure": "primary","Amazon Web": "danger","Google Cloud": "info","Fresher": "warning"}
        var storageRef = firebase.storage().ref();
        var imageRef = storageRef.child(`${this.$route.params.id}/resume/${this.form.email}`);
        // eslint-disable-next-line no-unused-vars
        const url = await imageRef.put(this.form.resume);
        var resume = await imageRef.getDownloadURL();
        const details = { email, experience, name: `${fname} ${lname}`, phone, resume, tags: {[areaLabel[area]]: area}, time: String(new Date()) };
        const actualPath = await firebase.firestore().collection("accounts").doc(this.$route.params.id);
        // eslint-disable-next-line no-unused-vars
        const res = await actualPath.update({"candidates.applied": firebase.firestore.FieldValue.arrayUnion(details),});
        this.showSuccessPopup = true
        setTimeout(() => {
          this.showSuccessPopup = null
        }, 3000);
      }
      else alert('Data Missing');
      this.loading = null;
    },
    handleFileUpload (func) {
      if (func === true) this.form.resume = this.$refs.file.files[0]
      else this.form.resume = null
    },
    async fetchRtdb() {
      try {
        const data = await DB.ref(`forms/${this.$route.params.id}`);
        data
          .get()
          .then((snapshot) => {
            if (snapshot.exists()) {
              this.formData = snapshot.val();
            } else {
              console.log("No data available");
            }
          })
          .catch((error) => {
            console.error(error);
          });
      } catch (err) {
        alert(err.message);
      }
      this.loading = null;
    },
  },
  created() {
    this.fetchRtdb();
  },
};
</script>
