<template>
  <header>
    <div class="container-fluid mt-3 hidden sm:block">
      <nav class="navbar">
        <!-- logo -->
        <ul class="navbar-nav">
          <div class="flex items-center">
            <div class="navbar__logo">
              <img src="../assets/image/VTP2_1.svg" alt="website logo" />
            </div>
            <div class="sm:block hidden xl:hidden only:">
              <div class="relative">
                <button
                  class="bg-transparent text-black text-3xl hover:bg-red-600"
                  @click="() => handleDrop('menu')"
                >
                  <font-awesome-icon :icon="['fas', 'bars']" />
                </button>
                <div
                  v-if="showDropDown"
                  class="absolute left-10 w-[300px] bg-[#f07276] text-white z-40 rounded-lg"
                >
                  <div class="nav-item m-4">
                    <router-link
                      :to="{ name: 'home', params: {} }"
                      class="nav-link text-white"
                      @click="() => handleDrop('item')"
                      >Home</router-link
                    >
                  </div>
                  <div class="nav-item m-4">
                    <a
                      href="#"
                      class="nav-link text-white"
                      @click="() => handleDrop('item')"
                      >Service</a
                    >
                  </div>
                  <div class="nav-item m-4">
                    <router-link
                      :to="{ name: 'Appointment', params: {} }"
                      class="nav-link text-white"
                      @click="() => handleDrop('item')"
                      >Make Appointment</router-link
                    >
                  </div>
                  <div class="nav-item m-4">
                    <a
                      href="#"
                      class="nav-link text-white"
                      @click="() => handleDrop('item')"
                      >Our Doctor</a
                    >
                  </div>
                  <div class="nav-item m-4">
                    <a
                      href="#"
                      class="nav-link text-white"
                      @click="() => handleDrop('item')"
                      >About Us</a
                    >
                  </div>
                </div>
              </div>
            </div>
          </div>
        </ul>
        <!-- links -->

        <template v-if="!isLoggedIn">
          <div class="hidden xl:block">
            <ul class="nav">
              <li class="nav-item">
                <router-link :to="{ name: 'home', params: {} }" class="nav-link"
                  >Home</router-link
                >
              </li>
              <li class="nav-item"><a href="#" class="nav-link">Service</a></li>
              <li class="nav-item">
                <router-link
                  :to="{ name: 'Appointment', params: {} }"
                  class="nav-link"
                  >Make Appointment</router-link
                >
              </li>
              <li class="nav-item">
                <router-link
                  :to="{ name: 'Doctor', params: {} }"
                  class="nav-link"
                  >Our Doctor</router-link
                >
              </li>
              <li class="nav-item">
                <a href="#" class="nav-link">About Us</a>
              </li>
            </ul>
          </div>
          <ul class="nav">
            <router-link
              :to="{ name: 'Login', params: {} }"
              class="nav-link"
              @click="isLogin"
              >Sign In</router-link
            >
            <router-link :to="{ name: 'Register', params: {} }">
              <button @click="isRegister">Register</button>
            </router-link>
          </ul>
        </template>
        <template v-else>
          <div class="hidden xl:block">
            <ul class="nav">
              <li class="nav-item">
                <router-link :to="{ name: 'home', params: {} }" class="nav-link"
                  >Home</router-link
                >
              </li>
              <li class="nav-item"><a href="#" class="nav-link">Service</a></li>
              <li class="nav-item">
                <router-link
                  :to="{ name: 'Appointment', params: {} }"
                  class="nav-link"
                  >Make Appointment</router-link
                >
              </li>
              <li class="nav-item">
                <a href="#" class="nav-link">Our Doctor</a>
              </li>
              <li class="nav-item">
                <a href="#" class="nav-link">About Us</a>
              </li>
            </ul>
          </div>

          <ul class="nav">
            <li class="nav-item">
              <router-link
                class="nav-link"
                :to="{ name: 'userProfile', params: {} }"
                >Hello {{ fullName }}</router-link
              >
            </li>
            <li class="nav-item">
              <button id="Logout" @click="logout">Logout</button>
            </li>
          </ul>
        </template>
      </nav>
    </div>
    <div class="sm:hidden">
      <div class="m-4">
        <img
          src="../assets/image/VTP2_1.svg"
          alt="website logo"
          class="w-[200px]"
        />
      </div>
      <div class="flex justify-between">
        <div class="relative">
          <button
            class="bg-transparent text-black text-3xl hover:bg-red-600"
            @click="() => handleDrop('menu')"
          >
            <font-awesome-icon :icon="['fas', 'bars']" />
          </button>
          <div
            v-if="showDropDown"
            class="absolute left-0 w-[314px] bg-[#f07276] text-white z-40 rounded-lg"
          >
            <div class="nav-item m-4">
              <router-link
                :to="{ name: 'home', params: {} }"
                class="nav-link text-white"
                @click="() => handleDrop('item')"
                >Home</router-link
              >
            </div>
            <div class="nav-item m-4">
              <a
                href="#"
                class="nav-link text-white"
                @click="() => handleDrop('item')"
                >Service</a
              >
            </div>
            <div class="nav-item m-4">
              <router-link
                :to="{ name: 'Appointment', params: {} }"
                class="nav-link text-white"
                @click="() => handleDrop('item')"
                >Make Appointment</router-link
              >
            </div>
            <div class="nav-item m-4">
              <a
                href="#"
                class="nav-link text-white"
                @click="() => handleDrop('item')"
                >Our Doctor</a
              >
            </div>
            <div class="nav-item m-4">
              <a
                href="#"
                class="nav-link text-white"
                @click="() => handleDrop('item')"
                >About Us</a
              >
            </div>
          </div>
        </div>
        <div>
          <div v-if="!isLoggedIn">
            <ul class="nav">
              <router-link
                :to="{ name: 'Login', params: {} }"
                class="nav-link"
                @click="isLogin"
                >Sign In</router-link
              >
              <router-link :to="{ name: 'Register', params: {} }">
                <button @click="isRegister">Register</button>
              </router-link>
            </ul>
          </div>
          <div v-else>
            <ul class="nav">
              <li class="nav-item">
                <router-link
                  class="nav-link"
                  :to="{ name: 'userProfile', params: {} }"
                  >Hello {{ fullName }}</router-link
                >
              </li>
              <li class="nav-item">
                <button id="Logout" @click="logout">Logout</button>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>
<script>
import { ref } from "vue";
export default {
  data() {
    return {
      fullName: localStorage.getItem("fullName"),
      showDropDown: ref(false),
    };
  },
  setup() {
    return {};
  },
  computed: {
    isLoggedIn() {
      return window.localStorage.getItem("fullName");
    },
  },
  methods: {
    logout() {
      window.localStorage.removeItem("token");
      window.localStorage.removeItem("fullName");
      window.localStorage.removeItem("role");
      window.localStorage.removeItem("usId");
      window.localStorage.removeItem("DoctorId");
      window.localStorage.removeItem("NurseId");
      window.localStorage.removeItem("PatientId");
      this.$router.go();
    },
    handleDrop() {
      this.showDropDown = !this.showDropDown;
    },
  },
};
</script>
