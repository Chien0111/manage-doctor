<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <div class="banner">
      <img :src="require('@/assets/image/Rectangle1146.jpg')" />
      <p class="text1">The best solution</p>
      <p class="text2">for all your health needs.</p>
    </div>
    <div class="body-title container text-3xl font-medium">
      <h1 class="text-center mt-4 mb-4">Why should you choose EGO?</h1>
    </div>
    <div class="body-content">
      <div class="flex flex-wrap justify-center">
        <div class="square d-flex flex-column align-items-center m-4">
          <div class="d-flex justify-content-center align-items-center">
            <img
              src="../assets/image/Vector.png"
              height="180px"
              class="hexagon position-absolute"
            />
            <img
              src="../assets/image/Group.png"
              class="hexagon position-absolute"
            />
          </div>
          <div class="mt-auto mb-5 d-flex justify-content-center">
            <button>See Details</button>
          </div>
        </div>
        <div class="square d-flex flex-column align-items-center m-4">
          <div class="d-flex justify-content-center align-items-center">
            <img
              src="../assets/image/Vector.png"
              height="180px"
              class="hexagon position-absolute"
            />
            <img
              src="../assets/image/Plurginphone.png"
              class="hexagon position-absolute"
            />
          </div>
          <div class="mt-auto mb-5 d-flex justify-content-center">
            <button>See Details</button>
          </div>
        </div>
        <div class="square d-flex flex-column align-items-center m-4">
          <div class="d-flex justify-content-center align-items-center">
            <img
              src="../assets/image/Vector.png"
              height="180px"
              class="hexagon position-absolute"
            />
            <img
              src="../assets/image/Khien.png"
              class="hexagon position-absolute"
            />
          </div>
          <div class="mt-auto mb-5 d-flex justify-content-center">
            <button>See Details</button>
          </div>
        </div>
        <div class="square d-flex flex-column align-items-center m-4">
          <div class="d-flex justify-content-center align-items-center">
            <img
              src="../assets/image/Vector.png"
              height="180px"
              class="hexagon position-absolute"
            />
            <img
              src="../assets/image/PlurginPricelist.png"
              class="hexagon position-absolute"
            />
          </div>
          <div class="mt-auto mb-5 d-flex justify-content-center">
            <button>See Details</button>
          </div>
        </div>
      </div>
    </div>
    <div class="body-title container">
      <h1 class="text-center mt-4 text-3xl font-medium">
        The best service at EGO
      </h1>
      <h5>We want to bring you the best healthcare experience</h5>
    </div>
    <div class="flex justify-center flex-wrap">
      <div
        v-for="item in dataService"
        :key="item"
        class="flex items-center justify-around border-[1px] border-t-zinc-300 rounded-lg p-6 shadow-card w-[360px] m-4"
      >
        <div>
          <h1 class="font-medium text-2xl">{{ item.title }}</h1>
          <ul>
            <li class="my-4">
              <font-awesome-icon
                :icon="['fass', 'circle']"
                class="text-[6px]"
              />
              {{ item.content }}
            </li>
          </ul>
          <button>+ more</button>
        </div>
        <div>
          <font-awesome-icon :icon="['fas', 'syringe']" class="text-4xl" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";
export default {
  setup() {
    const dataService = ref([
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
      {
        title: "Addiction Medicine",
        content: "Confidential counseling",
      },
    ]);
    return {
      dataService,
    };
  },
};
</script>
<style scoped src="../assets/style/home.css"></style>
