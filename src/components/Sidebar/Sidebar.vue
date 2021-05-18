<template>
  <nav
  v-if="companyData"
    class="md:left-0 md:block md:fixed md:top-0 md:bottom-0 md:overflow-y-auto md:flex-row md:flex-nowrap md:overflow-hidden shadow-xl bg-white flex flex-wrap items-center justify-between relative md:w-64 z-10 py-4 px-6"
  >
    <div
      class="md:flex-col md:items-stretch md:min-h-full md:flex-nowrap px-0 flex flex-wrap items-center justify-between w-full mx-auto"
    >
      <!-- Brand -->
      <router-link
        class="md:block text-left text-blueGray-600 mr-0 inline-block whitespace-nowrap text-sm uppercase font-bold p-2 px-0"
        to="/"
      >
        {{companyData.user.name}}
      </router-link>
      <div
        class="md:flex md:flex-col md:items-stretch md:opacity-100 md:relative md:mt-4 md:shadow-none shadow absolute top-0 left-0 right-0 z-40 overflow-y-auto overflow-x-hidden h-auto items-center flex-1 rounded"
        v-bind:class="collapseShow"
      >
        <!-- Collapse header -->
        <div
          class="md:min-w-full md:hidden block pb-4 mb-4 border-b border-solid border-blueGray-200"
        >
          <div class="flex flex-wrap">
            <div class="w-6/12">
              <router-link
                class="md:block text-left md:pb-2 text-blueGray-600 mr-0 inline-block whitespace-nowrap text-sm uppercase font-bold p-4 px-0"
                to="/"
              >
                Vue Notus
              </router-link>
            </div>
            <div class="w-6/12 flex justify-end">
              <button
                type="button"
                class="cursor-pointer text-black opacity-50 md:hidden px-3 py-1 text-xl leading-none bg-transparent rounded border border-solid border-transparent"
                v-on:click="toggleCollapseShow('hidden')"
              >
                <i class="fas fa-times"></i>
              </button>
            </div>
          </div>
        </div>

        <!-- Divider -->
        <hr class="mb-4 md:min-w-full" />
        <!-- Heading -->
        <h6
          class="md:min-w-full text-blueGray-500 text-xs uppercase font-bold block pb-4 no-underline"
        >
          General
        </h6>
        <!-- Navigation -->

        <ul class="md:flex-col md:min-w-full flex flex-col list-none" v-for="(item, index) in general" :key="index">
          <li class="items-center">
            <router-link
              :to="`/admin/${item.path}`"
              v-slot="{ href, navigate, isActive }"
            >
              <a
                :href="href"
                @click="navigate"
                class="text-xs uppercase py-3 font-bold block"
                :class="[
                  isActive
                    ? 'text-emerald-500 hover:text-emerald-600'
                    : 'text-blueGray-700 hover:text-blueGray-500',
                ]"
              >
                <i
                  class="mr-2 text-sm"
                  :class="`fas fa-${item.icon}`"
                ></i>
                {{item.name}}
              </a>
            </router-link>
          </li>
        </ul>
        <!-- Divider -->
        <hr class="my-4 md:min-w-full" />
        <!-- Heading -->
        <h6 v-if="codingRound.length"
          class="md:min-w-full text-blueGray-500 text-xs uppercase font-bold block pb-4 no-underline"
        >
          Coding Section
        </h6>
        <!-- Navigation -->

        <ul class="md:flex-col md:min-w-full flex flex-col list-none" v-for="(item, index) in codingRound" :key="index">
          <li class="items-center">
            <router-link
              :to="`/admin/coding/${item.path}`"
              v-slot="{ href, navigate, isActive }"
            >
              <a
                :href="href"
                @click="navigate"
                class="text-xs uppercase py-3 font-bold block"
                :class="[
                  isActive
                    ? 'text-emerald-500 hover:text-emerald-600'
                    : 'text-blueGray-700 hover:text-blueGray-500',
                ]"
              >
                <i
                  class="mr-2 text-sm"
                  :class="`fas fa-${item.icon}`"
                ></i>
                {{item.name}}
              </a>
            </router-link>
          </li>
        </ul>

        <!-- Divider -->
        <hr class="my-4 md:min-w-full" />
        <!-- Heading -->
        <h6
          class="md:min-w-full text-blueGray-500 text-xs uppercase font-bold block pb-4 no-underline"
        >
          Support
        </h6>
        <!-- Navigation -->

        <ul class="md:flex-col md:min-w-full flex flex-col list-none">
          <li class="items-center">
            <router-link
              to="/admin/ticket"
              v-slot="{ href, navigate, isActive }"
            >
              <a
                :href="href"
                @click="navigate"
                class="text-xs uppercase py-3 font-bold block"
                :class="[
                  isActive
                    ? 'text-emerald-500 hover:text-emerald-600'
                    : 'text-blueGray-700 hover:text-blueGray-500',
                ]"
              >
                <i
                  class="fas fa-ticket-alt mr-2 text-sm"
                  :class="[isActive ? 'opacity-75' : 'text-blueGray-300']"
                ></i>
                Raise Ticket
              </a>
            </router-link>
          </li>

          <li class="items-center">
            <router-link
              to="/"
              v-slot="{ href, navigate, isActive }"
            >
              <a
                :href="href"
                @click="navigate"
                class="text-xs uppercase py-3 font-bold block"
                :class="[
                  isActive
                    ? 'text-emerald-500 hover:text-emerald-600'
                    : 'text-blueGray-700 hover:text-blueGray-500',
                ]"
              >
                <i
                  class="fas fa-sign-out-alt mr-2 text-sm"
                  :class="[isActive ? 'opacity-75' : 'text-blueGray-300']"
                ></i>
                Close Test
              </a>
            </router-link>
          </li>
        </ul>

        
      </div>
    </div>
  </nav>
</template>
); }

<script>

export default {
  data() {
    return {
      collapseShow: "hidden",
    };
  },
  props: {
    companyData: Object
  },
  computed : {
    general () {
      const categories = Object.keys(this.companyData.assignment.tags)
      const generalArray = []
      categories.forEach((e) => {
        if (e === 'mcq') generalArray.push({name: 'MCQs', path: 'mcq', icon: 'list'})
        if (e === 'regex') generalArray.push({name: 'Regex Pattern', path: 'regex', icon: 'fingerprint'})
        if (e === 'rest') generalArray.push({name: 'REST APIs', path: 'rest', icon: 'cloud-upload-alt'})
      })
      return generalArray.sort((a, b) => a.name.localeCompare(b.name))
    },
    codingRound () {
      const categories = Object.keys(this.companyData.assignment.tags)
      const generalArray = []
      categories.forEach((e) => {
        if (e === 'array') generalArray.push({name: 'Array', path: 'array', icon: 'code'})
        if (e === 'dp') generalArray.push({name: 'Dynamic Programming', path: 'dp', icon: 'code'})
      })
      return generalArray.sort((a, b) => a.name.localeCompare(b.name))
    }
  },
  methods: {
    toggleCollapseShow: function (classes) {
      this.collapseShow = classes;
    },
  },
};
</script>
