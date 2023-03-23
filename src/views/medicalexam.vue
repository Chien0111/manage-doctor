<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <div class="flex flex-wrap xl:flex-nowrap justify-center">
      <div class="w-full xl:w-[240px]" style="margin-right: 5px">
        <div class="mb-3">
          <Listbox
            v-model="selectedPatient"
            :options="queue"
            :filter="true"
            optionLabel="patient.user.fullName"
            listStyle="max-height:250px"
            style="width: 100%"
            filterPlaceholder="Search"
            @click="setData"
          >
            <template #option="item">
              <div class="country-item">
                <div>
                  {{ item.option.patient.user.fullName }}
                </div>
              </div>
            </template>
          </Listbox>
        </div>
        <div>
          <fieldset>
            <legend><h5>Medical Record</h5></legend>
            <div class="fieldset-content">
              <Listbox
                v-model="selectedPreviousPresciption"
                listStyle="max-height:250px"
                :options="previousPresciption"
                optionLabel="createdDate"
              >
                <template #option="item">
                  <div class="country-item">
                    <div
                      class="text-center"
                      @click="openPrescriptionDialog(item)"
                    >
                      {{ this.dateToYMD(item.option.createdDate) }}
                    </div>
                  </div>
                </template>
              </Listbox>
            </div>
          </fieldset>
          <div
            style="
              align-items: center;
              flex-direction: column;
              justify-content: space-around;
              display: flex;
            "
          >
            <button
              id="saveAndPrintBtn"
              data-bs-toggle="modal"
              data-bs-target="#printModal"
              class="btn btn-success mt-2"
              style="margin-bottom: 20px"
              :disabled="
                this.selectedPatient === null || this.prescription.length === 0
              "
            >
              Save and Print
            </button>
          </div>
        </div>
      </div>
      <div class="w-full xl:w-4/5">
        <div class="mb-3" style="width: 104%">
          <fieldset>
            <legend><h5>Patient Health Tracking Status</h5></legend>
            <div class="fieldset-content">
              <div class="d-flex justify-content-between flex-wrap">
                <div>
                  <span> Full Name:</span>
                  <InputText
                    v-model="patientHealthTracking.patient.user.fullName"
                    type="text"
                    class="form-control"
                    readonly
                  />
                </div>
                <div>
                  <span> DoB:</span>
                  <InputText
                    type="date"
                    v-model="patientHealthTracking.patient.user.birthDay"
                    class="form-control"
                    readonly
                  />
                </div>
                <div>
                  <span> Gender:</span>
                  <InputText
                    type="text"
                    v-model="patientHealthTracking.patient.user.gender"
                    class="form-control"
                    readonly
                  />
                </div>
                <div>
                  <span> Address:</span>
                  <InputText
                    type="text"
                    v-model="patientHealthTracking.patient.user.address"
                    class="form-control"
                    readonly
                  />
                </div>
                <div>
                  <span>Phone Number:</span>
                  <InputText
                    type="text"
                    v-model="patientHealthTracking.patient.user.phoneNumber"
                    class="form-control"
                    readonly
                  />
                </div>
              </div>
              <div class="d-flex justify-content-between flex-wrap">
                <div>
                  <span>Pulse:</span>
                  <InputNumber
                    inputId="withoutgrouping"
                    v-model="patientHealthTracking.pulse"
                    mode="decimal"
                    :useGrouping="false"
                    placeholder="bpm"
                    class="form-control"
                  />
                </div>
                <div>
                  <span>Blood Pressure:</span>
                  <InputNumber
                    inputId="withoutgrouping"
                    v-model="patientHealthTracking.bloodPressure"
                    mode="decimal"
                    :useGrouping="false"
                    placeholder="mmHg"
                    class="form-control"
                  />
                </div>
                <div>
                  <span>Temperature:</span>
                  <InputNumber
                    inputId="withoutgrouping"
                    v-model="patientHealthTracking.tempurature"
                    mode="decimal"
                    :useGrouping="false"
                    placeholder="°C"
                    class="form-control"
                  />
                </div>
                <div>
                  <span>Weight:</span>
                  <InputNumber
                    inputId="withoutgrouping"
                    v-model="patientHealthTracking.weight"
                    mode="decimal"
                    :useGrouping="false"
                    placeholder="Kg"
                    class="form-control"
                  />
                </div>
                <div>
                  <span>Height:</span>
                  <InputNumber
                    inputId="withoutgrouping"
                    v-model="patientHealthTracking.height"
                    mode="decimal"
                    :useGrouping="false"
                    placeholder="Cm"
                    class="form-control"
                  />
                </div>
              </div>
            </div>
          </fieldset>
        </div>

        <div class="mb-3" style="width: 104%">
          <fieldset style="height: 550px">
            <legend>
              <h1 class="w-full mb-4 text-3xl">Presciption:</h1>
              <div
                class="d-flex flex-wrap justify-content-between align-items-center"
              >
                <div class="lg:w-1/3">
                  <h5 class="me-2">Using Day</h5>
                  <InputNumber mode="decimal" class="w-full m-2" />
                </div>

                <div class="lg:w-1/3">
                  <h5 class="me-2">Re-Examination day</h5>
                  <Calendar dateFormat="mm-dd-yy" class="w-full m-2" />
                </div>
                <div>
                  <Button
                    label="Clear Presciption"
                    class="p-button-rounded p-button-danger my-2"
                    @click="emptyMedicineInPrescription()"
                  />
                </div>
              </div>
            </legend>
            <div class="fieldset-content">
              <DataTable
                :value="prescription"
                :scrollable="true"
                scrollHeight="400px"
                :loading="loading"
              >
                <template #empty> No project found. </template>
                <template #loading>
                  <ProgressSpinner />
                </template>
                <Column field="medicineName" header="Medicine Name">
                  <template #body="{ data }">
                    {{ data.medicineName }}
                  </template>
                </Column>
                <Column header="Amount">
                  <template #body="{ data }">
                    <input
                      type="number"
                      class="form-control"
                      v-model="data.amount"
                    />
                  </template>
                </Column>
                <Column header="Using Day">
                  <template #body="{ data }">
                    <input
                      type="number"
                      class="form-control"
                      v-model="data.usingDay"
                    />
                  </template>
                </Column>
                <Column header="Times Per Day">
                  <template #body="{ data }">
                    <input
                      type="number"
                      class="form-control"
                      v-model="data.timesPerDay"
                    />
                  </template>
                </Column>
                <Column header="Using Type">
                  <template #body="{ data }">
                    <input
                      type="number"
                      class="form-control"
                      v-model="data.usingType"
                    />
                  </template>
                </Column>
                <Column header="Session">
                  <template #body>
                    <Dropdown
                      v-model="selectedSession"
                      :options="session"
                      optionLabel="name"
                      placeholder="Session"
                    />
                  </template>
                </Column>
                <Column header="Action">
                  <template #body="{ data, index }">
                    <Button
                      label="Delete"
                      class="p-button-danger"
                      @click="deleteMedicineInPrescription(index, data)"
                    />
                  </template>
                </Column>
              </DataTable>
            </div>
          </fieldset>
        </div>
        <div style="width: 104%">
          <DataTable
            :value="medicines"
            :scrollable="true"
            scrollHeight="400px"
            :loading="loading"
            showGridlines
            id="prescriptionTable"
          >
            <template #header>
              <div class="d-flex justify-content-between align-items-center">
                <h5 class="m-0">Medicine</h5>
                <span class="p-input-icon-left">
                  <i class="pi pi-search" />
                  <InputText placeholder="Keyword Search" />
                </span>
              </div>
            </template>
            <Column field="medicineName" header="Medicine Name">
              <template #body="{ data }">
                {{ data.medicineName }}
              </template>
            </Column>
            <Column field="price" header="Price">
              <template #body="{ data }">
                {{ data.price }}
              </template>
            </Column>
            <Column field="inStock" header="In Stock">
              <template #body="{ data }">
                {{ data.inStock }}
              </template>
            </Column>
            <Column header="Add Medicine">
              <template #body="{ data, index }">
                <Button
                  label="Add Medicine"
                  class="btnAddMedicine p-button-raised p-button-warning"
                  @click="addRowMethod(index, data)"
                />
              </template>
            </Column>
            <template #empty> No project found. </template>
            <template #loading>
              <ProgressSpinner />
            </template>
          </DataTable>
        </div>
      </div>
    </div>
    <div
      class="modal fade"
      id="printModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Prescription</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div ref="image">
            <div class="modal-body" id="prescription">
              <div class="d-flex justify-content-between">
                <div>
                  <img
                    :src="require(`@/assets/image/VTP2_1.svg`)"
                    style="width: 100px"
                  />
                </div>
                <div>
                  <p style="margin-top: 10px; color: gray">
                    Presciption No #001
                  </p>
                </div>
              </div>
              <hr style="margin-top: 0px" />
              <div class="d-flex justify-content-between prescriptionContent">
                <div>
                  <b>Ego Clinic</b>
                  <p>Specialist clinic</p>
                  <p>Doctor: {{ this.doctorName }}</p>
                  <p>TelePhone: 0909090090</p>
                </div>
                <div>
                  <p class="text-end">Fpt University</p>
                  <p class="text-end"><b>Working time</b></p>
                  <p class="text-end">Monday to Friday</p>
                  <p class="text-end">7:00 am to 6:00 pm</p>
                </div>
              </div>
              <h2 class="text-center mt-3">Presciption</h2>
              <div class="d-flex justify-content-between mt-3">
                <small
                  >Patient Name: <b>{{ this.fullName }}</b>
                </small>
                <small
                  >Gender: <b>{{ this.gender }}</b>
                </small>
                <small
                  >Age: <b>{{ this.Age }}</b>
                </small>
                <small
                  >Address: <b>{{ this.address }} </b>
                </small>
              </div>
              <div>
                <table class="table table-striped">
                  <thead>
                    <tr>
                      <th>#</th>
                      <th>Medicine name</th>
                      <th>Time per Days</th>
                      <th>using Day</th>
                      <th>Method</th>
                      <th>Session</th>
                    </tr>
                  </thead>
                  <tbody id="PresciptionTbody">
                    <tr v-for="(item, index) in prescription" :key="index">
                      <td>{{ ++index }}</td>
                      <td>{{ item.medicineName }}</td>
                      <td>{{ item.usingTime }}</td>
                      <td>{{ item.timesPerDay }}</td>
                      <td>{{ item.usingType }}</td>
                      <td>{{ item.usingType }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <div class="mt-5 bottom-0 start-50 row">
                <p>
                  <b
                    >Seek immediate medical attention if patient
                    <ins>has any</ins> of the following symptoms:</b
                  >
                </p>
                <div class="col-md-4">
                  <p>- Convulsions</p>
                  <p>- Sound asleep</p>
                </div>
                <div class="col-md-4">
                  <p>- Vomiting</p>
                  <p>- Persistent high fever</p>
                </div>
                <div class="col-md-4">
                  <p>- Tired breathing</p>
                  <p>- Bloody diarrhea</p>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <button @click="printImage()" type="button" class="btn btn-primary">
              Print
            </button>
          </div>
        </div>
      </div>
    </div>
    <Dialog
      v-model:visible="prescriptionDialog"
      modal
      :style="{ width: '50vw' }"
    >
      <template #header> <h5>Presciption Details</h5></template>
      <div class="modal-body" id="prescription">
        <div class="d-flex justify-content-between">
          <div>
            <img
              :src="require(`@/assets/image/VTP2_1.svg`)"
              style="width: 100px"
            />
          </div>
          <div>
            <p style="margin-top: 10px; color: gray">
              Presciption No #{{ this.previousPresciption[0].appointmentId }}
            </p>
          </div>
        </div>
        <hr style="margin-top: 0px" />
        <div class="d-flex justify-content-between prescriptionContent">
          <div>
            <b>Ego Clinic</b>
            <p>Specialist clinic</p>
            <p>Doctor: {{ this.doctorName }}</p>
            <p>TelePhone: 0909090090</p>
          </div>
          <div>
            <p class="text-end">Fpt University</p>
            <p class="text-end"><b>Working time</b></p>
            <p class="text-end">Monday to Friday</p>
            <p class="text-end">7:00 am to 6:00 pm</p>
          </div>
        </div>
        <h2 class="text-center mt-3">Presciption</h2>
        <div class="d-flex justify-content-between mt-3">
          <small
            >Patient Name:
            <b>{{ this.selectedPatient.patient.user.fullName }}</b>
          </small>
          <small
            >Gender: <b>{{ this.selectedPatient.patient.user.gender }}</b>
          </small>
          <small
            >Age:
            <b>{{
              this.caculateAge(this.selectedPatient.patient.user.birthDay)
            }}</b>
          </small>
          <small
            >Address: <b>{{ this.selectedPatient.patient.user.address }} </b>
          </small>
        </div>
        <div>
          <table class="table table-striped">
            <thead>
              <tr>
                <th>#</th>
                <th>Medicine name</th>
                <th>Time per Days</th>
                <th>using Day</th>
                <th>Method</th>
                <th>Session</th>
              </tr>
            </thead>
            <tbody id="PresciptionTbody">
              <tr
                v-for="(item, index) in this.previousPresciption[0]
                  .prescriptionDetails"
                :key="index"
              >
                <td>{{ ++index }}</td>
                <td>{{ item.medicine.medicineName }}</td>
                <td>{{ item.timesPerDay }}</td>
                <td>{{ item.usingDay }}</td>
                <td>{{ item.usingType }}</td>
                <td>{{ item.session }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="mt-5 bottom-0 start-50 row">
          <p>
            <b
              >Seek immediate medical attention if patient <ins>has any</ins> of
              the following symptoms:</b
            >
          </p>
          <div class="col-md-4">
            <p>- Convulsions</p>
            <p>- Sound asleep</p>
          </div>
          <div class="col-md-4">
            <p>- Vomiting</p>
            <p>- Persistent high fever</p>
          </div>
          <div class="col-md-4">
            <p>- Tired breathing</p>
            <p>- Bloody diarrhea</p>
          </div>
        </div>
      </div>
    </Dialog>
  </div>
</template>
<style scoped>
#patientBody {
  text-align: center;
  display: block;
  height: 150px;
  overflow-y: scroll;
}
.disable {
  pointer-events: none;
  border: 1px solid #999999;
  background-color: #cccccc;
  color: #666666;
}
.pdf-container {
  height: 500px;
}
fieldset {
  box-sizing: border-box;
  border: 1px solid #dee2e6;
  background: #ffffff;
  color: #495057;
  border-radius: 6px;
}
fieldset legend {
  padding: 1.25rem;
  border: 1px solid #dee2e6;
  color: #343a40;
  background: #f4d7d3;
  font-weight: 700;
  border-radius: 6px;
}
.fieldset-content {
  box-sizing: border-box;
  padding: 1.25rem;
}
</style>
<style>
.p-listbox .p-listbox-header {
  background-color: #f8d7d3;
}
.listbox-header {
  background-color: #f8d7d3;
}
.p-listbox-list-wrapper {
  height: 250px;
}
.p-listbox .p-listbox-list .p-listbox-item {
  border-bottom: 1px solid #cfd8dc;
}
.p-datatable.p-datatable-gridlines .p-datatable-header {
  background-color: #f8d7d3;
}
.p-inputnumber.p-component.p-inputwrapper.form-control {
  border: none;
}
.p-inputnumber.p-component.p-inputwrapper {
  margin-left: -10px;
}
</style>
<script>
import { HTTP } from "@/axios";
import dayjs from "dayjs";
import html2canvas from "html2canvas";
import { HubConnectionBuilder, LogLevel } from "@microsoft/signalr";
export default {
  data() {
    return {
      connection: null,
      queue: [],
      medicines: [],
      prescription: [],
      doctorName: localStorage.getItem("fullName"),
      loading: false,
      selectedPatient: null,
      patientHealthTracking: {
        bloodPressure: null,
        doctorId: null,
        height: null,
        patient: {
          id: null,
          user: {
            address: "",
            birthDay: "",
            email: "",
            fullName: "",
            gender: "",
            phoneNumber: "",
          },
        },
        patientId: null,
        pulse: null,
        tempurature: null,
        weight: null,
      },
      previousPresciption: null,
      prescriptionDialog: false,
      session: [
        { id: 1, value: 1, name: "Morning" },
        { id: 2, value: 2, name: "Noon" },
        { id: 3, value: 3, name: "Afternoon" },
      ],
      selectedSession: null,
    };
  },
  created() {
    this.getMedicine();
    this.connection = new HubConnectionBuilder()
      .withUrl("https://localhost:5001/SignalRServer")
      .configureLogging(LogLevel.Information)
      .build();
    this.connection
      .start()
      .then(() => {
        this.getQueue();
        console.log("Connection started.");
        this.connection.on("ReceiveQueue", () => {
          this.getQueue();
        });
      })
      .catch((error) => console.error(error));
  },
  // watch: {
  //   prescription: {
  //     handler: function Change(text) {
  //       console.log(text);
  //     },
  //   },
  //   deep: true,
  // },
  methods: {
    caculateAge(date) {
      return dayjs(new Date()).get("year") - dayjs(date).get("year");
    },
    formatDate(date) {
      return dayjs(date).format("YYYY-MM-DD HH:mm");
    },
    async getQueue() {
      // eslint-disable-next-line no-unused-vars
      HTTP.get("Queue", {
        params: {
          doctorId: window.localStorage.getItem("DoctorId"),
        },
      })
        .then((response) => {
          this.queue = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    async getMedicine() {
      // eslint-disable-next-line no-unused-vars
      this.loading = true;
      HTTP.get("Medicines/GetAllMedicines")
        .then((response) => {
          this.medicines = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
      this.loading = false;
    },
    setData() {
      const clickedItem = this.selectedPatient;
      this.patientHealthTracking = { ...this.selectedPatient };
      this.patientHealthTracking.patient.user.birthDay = this.dateToYMD(
        this.patientHealthTracking.patient.user.birthDay
      );
      this.getPreviousPresciption();
      if (clickedItem) {
        console.log(clickedItem);
      }
    },
    dateToYMD(end_date) {
      var ed = new Date(end_date);
      var d = ed.getDate();
      var m = ed.getMonth() + 1;
      var y = ed.getFullYear();
      return (
        "" + y + "-" + (m <= 9 ? "0" + m : m) + "-" + (d <= 9 ? "0" + d : d)
      );
    },
    addRowMethod(index, item) {
      const objWithIdIndex = this.medicines.findIndex(
        (obj) => obj.id === item.id
      );
      var element =
        document.getElementsByClassName("btnAddMedicine")[objWithIdIndex];
      element.classList.add("disable");
      var data = {
        amount: item.amount,
        description: item.description,
        id: item.id,
        medicineName: item.medicineName,
        usingTime: null,
        timesPerDay: null,
        usingType: null,
        session: null,
      };
      this.prescription.push(data);
      console.log(this.prescription);
    },
    deleteMedicineInPrescription(index, item) {
      const objWithIdIndex = this.prescription.findIndex(
        (obj) => obj.id === item.id
      );

      if (objWithIdIndex > -1) {
        this.prescription.splice(objWithIdIndex, 1);
      }
      const objWithIdIndex1 = this.medicines.findIndex(
        (obj) => obj.id === item.id
      );

      var element =
        document.getElementsByClassName("btnAddMedicine")[objWithIdIndex1];
      element.classList.remove("disable");
    },
    emptyMedicineInPrescription() {
      this.prescription = [];
      var itemClass = document.querySelectorAll(".btnAddMedicine");
      itemClass.forEach((el) => {
        el.classList.remove("disable");
      });
    },
    async getPreviousPresciption() {
      await HTTP.get(
        "Prescription/GetAllPrescriptionsByPatientId/" +
          this.patientHealthTracking.patient.id
      )
        .then((res) => {
          this.previousPresciption = res.data;
          console.log(this.previousPresciption);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    openPrescriptionDialog() {
      this.prescriptionDialog = true;
    },
    async printImage() {
      const elementToPrint = this.$refs.image;
      const canvas = await html2canvas(elementToPrint);

      const imageData = canvas.toDataURL("image/png");

      const image = new Image();
      image.src = imageData;

      const windowContent = "<!DOCTYPE html>";
      const printWindow = window.open("", "", "height=600,width=800");
      printWindow.document.write(windowContent);
      setTimeout(() => {
        printWindow.document.write(`
    <html>
      <head>
        <title>Print Prescription</title>
      </head>
      <body>
        <img src="${imageData}" />
      </body>
    </html>
  `);
        printWindow.document.close();
        printWindow.focus();
        printWindow.print();
        printWindow.close();
      }, 1000);
    },
  },
};
</script>
