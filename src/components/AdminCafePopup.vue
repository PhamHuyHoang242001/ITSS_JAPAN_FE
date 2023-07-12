<template>
  <div class="popup">
    <div class="popup-inner" @click="TogglePopup1()">
      <div class="flex justify-between pb-7 w-full">
        <div class="flex">
          <div class="w-10 h-10 self-center">
            <img
              src="https://cdn-icons-png.flaticon.com/128/1144/1144760.png"
              alt=""
            />
          </div>
          <div class="ml-5">
            <div class="font-semibold text-lg">{{ store.user["name"] }}</div>
            <div>承認が送信されました</div>
          </div>
        </div>
        <div>
          <div class="flex">
            <div>
              <button
                class="bg-[#FF4848] text-white hover:bg-red-600 font-semibold rounded-lg px-5 py-2"
              >
                Close
              </button>
            </div>
          </div>
        </div>
      </div>
      <div>
        <div class="font-semibold text-3xl pb-4">{{ store.name }}</div>
        <div class="flex">
          <div class="w-4 h-4 self-center">
            <img
              class="rounded-full cursor-pointer"
              src="https://cdn-icons-png.flaticon.com/128/2838/2838912.png"
              alt=""
            />
          </div>
          <div class="ml-4">
            {{ store.address }}
          </div>
        </div>
        <div class="flex">
          <div class="w-4 h-4 self-center">
            <img
              class="rounded-full cursor-pointer"
              src="https://cdn-icons-png.flaticon.com/128/3239/3239945.png"
              alt=""
            />
          </div>
          <div class="ml-4">
            {{ store.time_open }} - {{ store.time_close }} 
          </div>
          <!-- <div class="font-semibold text-sm">オープン中</div> -->
        </div>
        <div class="flex pb-3">
          <div class="w-4 h-4 self-center">
            <img
              src="https://cdn-icons-png.flaticon.com/128/159/159832.png"
              alt=""
            />
          </div>
          <div class="ml-4">{{ store.phone_number }}</div>
        </div>
        <p style="font-style:italic; font-size: 14px;" v-if="store.air_conditioner">エアコン：効いている</p>
      <p style="font-style: italic; font-size: 14px;" v-else>エアコン：効いていない</p>
        <div class="font-semibold text-xl">写真</div>
      </div>
      <div class="flex overflow-x-auto">
        <img
          v-for="(photo, index) in store.photoUrl"
          :src="photo.photoUrl"
          alt="Cafe Image"
          :key="index"
          class="rounded-lg aspect-w-1 aspect-h-1 max-w-xs mr-5"
          style="width: 400px; height: 200px"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      store: {
        id: this.storeDetail,
        name: "",
        address: "",
        city: "",
        phone_number: "",
        time_open: "",
        time_closed: "",
        photoUrl: [],
        air_conditioner: "",
        approve: "",
        user_id: "",
        star: "",
        bookmark: "",
        isOpen: "",
        user:""
      },
    };
  },
  created() {
    this.getDetail();
  },
  methods: {
    getDetail: function () {
      console.log(this.storeDetail);
      axios
        .get("/shop/show/" + this.storeDetail)
        .then((response) => {
          console.log(response);
          this.store = response.data.data;
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
  props: ["TogglePopup1", "storeDetail"],
};
</script>

<style scoped>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: rgba(0, 0, 0, 0.2);

  display: flex;
  align-items: center;
  justify-content: center;
}
.popup .popup-inner {
  height: 600px;
  width: 1000px;
  background: #fff;
  padding: 32px;
  border-radius: 14px;
  border: 2px solid black;
}
</style>
