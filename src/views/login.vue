<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <!-- Form Right -->
  <div class="m-4">
    <div class="d-flex justify-content-center">
      <h2 class="lg:text-4xl text-3xl m-4">Sign In</h2>
    </div>
    <input type="text" v-model="loginData.userName" placeholder="Email" />
    <input
      type="password"
      v-model="loginData.password"
      placeholder="Password"
    />
    <a
      href="#"
      class="text-black text-end d-block m-2"
      style="margin-top: -10px; margin-right: 20px"
      >Need help?</a
    >
    <div class="d-flex justify-content-center">
      <button class="confrim w-50" @click="handleLogin()">Submit</button>
    </div>
  </div>
</template>
<style src="../assets/style/authentication.css" scoped></style>
<script>
import { HTTP } from "@/axios";
import { ref } from "vue";
export default {
  setup() {
    const loginData = ref({
      userName: "",
      password: "",
    });
    return { loginData };
  },
  methods: {
    async handleLogin() {
      HTTP.post("Authentication/Login", {
        userName: this.loginData.userName,
        password: this.loginData.password,
      })
        .then((response) => {
          localStorage.setItem("token", response.data.token);
          localStorage.setItem("fullName", response.data.fullName);
          localStorage.setItem("role", response.data.role);
          if (response.data.role == "Doctor") {
            localStorage.setItem("DoctorId", response.data.doctorId);
          }
          if (response.data.role == "Nurse") {
            localStorage.setItem("NurseId", response.data.nurseId);
          }
          if (response.data.role == "Patient") {
            localStorage.setItem("PatientId", response.data.patientId);
          }
          localStorage.setItem("usId", response.data.id);
          this.$router.push({ name: "home", params: {} });
          this.showSuccess();
        })
        .catch((err) => {
          console.log(err);
          this.showError(err.response.data);
        });
    },
    showSuccess() {
      this.$toast.add({
        severity: "success",
        summary: "Success Message",
        detail: "Message Content",
        life: 3000,
      });
    },
    showError(err) {
      this.$toast.add({
        severity: "error",
        summary: "Login Fail",
        detail: err,
        life: 3000,
      });
    },
  },
};
</script>
