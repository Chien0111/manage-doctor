<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <Toast />
  <ConfirmDialog></ConfirmDialog>
  <div class="body">
    <sidebar :isActive="isActive" @open="open(isActive)" />
    <div
      class="page-content p-5"
      id="content"
      :class="{ active: isActive == true }"
    >
      <div class="mb-4 d-flex justify-content-between">
        <!-- Toggle button -->
        <div class="me-1">
          <button
            @click="open"
            id="sidebarCollapse"
            type="button"
            class="btn btn-light bg-white rounded-pill shadow-sm px-4 mb-4"
          >
            <i class="fa fa-bars mr-2"></i
            ><small class="text-uppercase font-weight-bold"></small>
          </button>
        </div>
        <div class="dropdown">
          <button
            type="button"
            id="dropdownMenuButton1"
            class="btn btn-light bg-white rounded-pill shadow-sm px-4"
            data-bs-toggle="dropdown"
            :aria-expanded="dropdown"
            :class="{ show: dropdown }"
            @click="openDropdown"
          >
            {{ fullName == "" ? "Ẩn Danh" : fullName
            }}<i class="fa-regular fa-user ms-1"></i>
          </button>
          <div>
            <ul
              class="dropdown-menu"
              :class="{ show: dropdown }"
              aria-labelledby="dropdownMenuButton1"
              style="width: 200px"
            >
              <router-link
                :to="{ name: 'Dashboard', params: {} }"
                style="color: black; text-decoration: none"
              >
                <li class="d-flex align-items-center">
                  <img
                    src="../assets/image/user.png"
                    height="30"
                    width="30"
                    class="me-3 ms-3"
                  />
                  <h5>{{ fullName }}</h5>
                </li>
              </router-link>

              <p class="text-center">{{ this.role }}</p>
              <hr />
              <li>
                <a
                  class="dropdown-item"
                  :href="$router.resolve({ name: 'home', params: {} }).href"
                  @click="logout"
                  >Logout</a
                >
              </li>
            </ul>
          </div>
        </div>
      </div>
      <slot />
    </div>
  </div>
</template>
<style src="../assets/style/admin.css" scoped></style>
<script>
import Sidebar from "@/components/SideBarAdminComponent.vue";
export default {
  components: { Sidebar },
  data() {
    return {
      isActive: false,
      fullName: localStorage.getItem("fullName"),
      dropdown: false,
      role: localStorage.getItem("role"),
    };
  },
  methods: {
    open() {
      this.isActive = !this.isActive;
    },
    logout() {
      window.localStorage.removeItem("token");
      window.localStorage.removeItem("fullName");
      window.localStorage.removeItem("role");
      window.localStorage.removeItem("usId");
      window.localStorage.removeItem("DoctorId");
      window.localStorage.removeItem("NurseId");
      window.localStorage.removeItem("PatientId");
    },
    openDropdown() {
      this.dropdown = !this.dropdown;
    },
  },
};
</script>
