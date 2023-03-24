<template>
  <div class="container">
    <div class="row">
      <div class="col-md-2">
        <!-- User profile picture -->
        <!-- <Image
          :src="require('@/assets/image/doctorImg.jpg')"
          alt="Image"
          preview
          class="card-top" save di
        /> -->
      </div>
      <div class="col-md-8">
        <!-- Profile editing form -->
        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <div class="flex justify-center">
              <div
                class="lg:w-[200px] lg:h-[200px] w-[150px] h-[150px] border-[1px] rounded-full m-6"
              >
                <img
                  src="https://vnn-imgs-f.vgcloud.vn/2020/03/23/11/trend-avatar-1.jpg"
                  class="object-cover w-full h-full rounded-full"
                  alt=""
                />
              </div>
            </div>
            <h3>
              Doctor {{ doctorInfo.user.firstName }}
              {{ doctorInfo.user.lastName }} Profile Settings
            </h3>
            <div class="container">
              <div class="row d-flex">
                <div class="col-md-6 mb-3">
                  <label>First Name</label>
                  <input
                    type="text"
                    class="form-control"
                    id="firstname"
                    placeholder="First Name"
                    v-model="doctorInfo.user.firstName"
                  />
                </div>
                <div class="col-md-6 mb-3">
                  <label>Last Name</label>
                  <input
                    type="text"
                    class="form-control"
                    id="lastname"
                    placeholder="Last Name"
                    v-model="doctorInfo.user.lastName"
                  />
                </div>
                <div class="col-md-6 mb-3">
                  <label>Date of Birth</label>
                  <Calendar
                    class="w-100"
                    id="birthday"
                    v-model="doctorInfo.user.birthDay"
                    dateFormat="dd/mm/yy"
                  />
                </div>
                <div class="col-md-6 mb-3">
                  <label>Gender</label>
                  <div class="flex flex-wrap gap-3">
                    <div class="form-check form-check-inline">
                      <RadioButton
                        v-model="doctorInfo.user.gender"
                        inputId="maleGender"
                        name="pizza"
                        value="Male"
                      />
                      <label class="ml-2">Male</label>
                    </div>
                    <div class="form-check form-check-inline mt-3">
                      <RadioButton
                        v-model="doctorInfo.user.gender"
                        inputId="femaleGender"
                        name="pizza"
                        value="Female"
                      />
                      <label class="ml-2">Female</label>
                    </div>
                    <div class="form-check form-check-inline">
                      <RadioButton
                        v-model="doctorInfo.user.gender"
                        inputId="otherGender"
                        name="pizza"
                        value="Undefine"
                      />
                      <label class="ml-2">Other</label>
                    </div>
                  </div>
                </div>
                <div class="col-md-6 mb-3">
                  <label>Email Address</label>
                  <input
                    type="email"
                    class="form-control"
                    id="email"
                    placeholder="Email Address"
                    v-model="doctorInfo.user.email"
                  />
                </div>
                <div class="col-md-6 mb-3">
                  <label>Phone Number</label>
                  <input
                    type="text"
                    class="form-control"
                    id="phonenum"
                    placeholder="Phone number"
                    v-model="doctorInfo.user.phoneNumber"
                  />
                </div>
                <div class="col-md-6 mb-3">
                  <label>User Name</label>
                  <input
                    type="text"
                    class="form-control"
                    id="username"
                    placeholder="User Name"
                    v-model="doctorInfo.user.userName"
                  />
                </div>
                <div class="col-md-6 mb-3">
                  <label>Address</label>
                  <input
                    type="text"
                    class="form-control"
                    id="address"
                    placeholder="Address"
                    v-model="doctorInfo.user.address"
                  />
                </div>
                <div class="col-md-12 mb-3">
                  <label>Experience</label>
                  <InputNumber
                    class="w-100"
                    prefix="Experience in "
                    suffix=" Year"
                    placeholder="Experience in ... Year"
                    v-model="doctorInfo.experience"
                  />
                </div>
                <div class="d-flex justify-content-end">
                  <button
                    class="btn btn-primary bg-white text-purple-600 px-4 border-[1px] border-purple-600 mr-4 rounded-lg hover:text-gray-400"
                    @click="back2home()"
                  >
                    Back To Home
                  </button>
                  <button
                    class="btn btn-primary bg-purple-500 border-none hover:bg-purple-600 text-white px-4"
                    @click="handleSubmit(this.doctorInfo.id)"
                  >
                    Save Changes
                  </button>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
      <div class="col md-2"></div>
    </div>
  </div>
</template>
<style>
.p-button-raised.p-button-danger {
  margin-left: 10px;
}
label {
  font-weight: bold;
}
form {
  background-color: white;
  padding: 20px;
  border-radius: 15px;
}
.p-dropdown.p-component.p-inputwrapper.p-inputwrapper-filled.form-control,
.p-inputtext.p-component {
  border-radius: 15px;
  height: 35px;
}

.form-control,
.p-inputtext.p-component {
  border-radius: 15px;
  height: 55px;
}

.p-image.p-component.card-top.p-image-preview-container img {
  width: 250px;
  height: 250px;
  border-radius: 500px /500px;
}

.p-image-preview-indicator {
  border-radius: 500px;
}
</style>
<script>
import { HTTP } from "@/axios";

// import { HTTP } from "@/axios";
export default {
  data() {
    return {
      doctorInfo: {
        id: null,
        experience: null,
        user: {
          firstName: null,
          lastName: null,
          gender: null,
          birthDay: null,
          address: null,
          email: null,
          userName: null,
          phoneNumber: null,
        },
      },

      selectGender: [
        { name: "Female", code: "0" },
        { name: "Male", code: "1" },
        { name: "Undefine", code: "2" },
      ],
    };
  },

  async created() {
    this.GetDocById();
  },
  methods: {
    async GetDocById() {
      this.loading = true;
      await HTTP.get(`Doctor/GetDoctorById?doctorId=1`).then((res) => {
        ((this.doctorInfo.user = { ...res.data.user }),
        (this.doctorInfo = { ...res.data })),
          console.log(this.doctorInfo);
      });
    },
    async handleSubmit(id) {
      this.$confirm.require({
        message: "Do you want to update your profile?",
        header: "Update User's Profile",
        icon: "fa fa-user-circle-o",
        acceptClass: "p-button-success",
        accept: async () => {
          HTTP.put("Doctor/EditDoctor?id=" + id, this.doctorInfo).then(
            (res) => {
              console.log(res);
              this.$toast.add({
                severity: "success",
                summary: "Confirmed",
                detail: "User have Updated Profile!",
                life: 3000,
              });
            }
          );
        },
        reject: () => {
          this.$toast.add({
            severity: "error",
            summary: "Rejected",
            detail: "User Rejected to Update Profile!",
            life: 3000,
          });
        },
      });
    },
  },
};
</script>
